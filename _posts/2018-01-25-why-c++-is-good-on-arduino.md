Today, there was [a question on arduino.stackexchange about c++ and arduinos with some disouraging answers about using c++][5]. It made me want to share some of my good expieriences with c++ on embedded devices.

C++ is fine with Arduinos or other AVR based Systems. I cant say much about pics, they have no decent gcc as far as i know.
I've been using c++ for quite a while in a home automation project. It made the code much more readable and maintainable. 

**What doesn't work**

The avr runtime does not support exceptions nor dynamic memory allocation, as has been stated. [you can provide an implementation  though][1].I didnt need it.

But there is much of C++ left to make use of. Here are some examples

**Templates**

They are very useful because, e.g. they allow to move work from runtime to compiletime. I use templates for a [replacement of the Arduino HAL functions DigitalWrite and others, without the runtime and code bloat][2]. 
The template based version is much more readable, and generates exactly the same assembler code. Please find the details in the other post.

**More on templates**

The following code uses the "curiously recurring template pattern" CRTP.
It is used to create a job class, which i use for a primitive scheduling algorithm.
This is a cooperative multitasking approach. The job classes work callback functions are called when it's their time to be called.

This template builds a chain of registrable items. I found this pattern somewhere in the internet - quite elegant.

    template<class registered>
    struct Registered {
    	static registered *registry;
    	registered *chain;
    	Registered() : chain(registry) {registry=static_cast<registered*>(this);}
    };

The job class applies the CRTP pattern. The constructor takes the actual callback where the work is done, and how often the job is done, and an initial delay. The run function will be called from the main loop regularly.

    class Job : public Registered<Job>
    {
    	voidFuncPtr m_p;
    	uint16_t m_periodic;
    	uint32_t m_lastrun;
    	uint16_t m_initialdelay;
    	public:
    	Job(voidFuncPtr p,uint16_t periode=0,uint16_t initialdelay=0):m_p(p),m_periodic(periode),m_lastrun(0),m_initialdelay(initialdelay)
    	{
    		
    	}
    	void run(uint32_t t_ms)
    	{
    		// tbd handle wraparound !!
    		if(t_ms >= m_lastrun + m_periodic + m_initialdelay)
    		{
    			m_p();
    			m_lastrun=t_ms;
    			m_initialdelay=0;
    		}
    	}
    };

to use the job class i just create the instances for the jobs and pass lambdas or regular function pointers. The code snippet is from my home automation project. 
There are three jobs, where the mqtt connection is polled every 10ms, the motor pid controller (for 4 valve motors) is called every 1ms, and a function which publishes the system state is called every 500ms.

    Job jserial([](){mqttrouteradapter.handleSerialMQTT();},10);
    Job jupdate(updatemc,1);
    Job jprint(printstate,500);

I initialize the beginning of the chain of jobs.

    template<> Job *Registered< Job >::registry = 0;

And inside main, run an infinite loop which calls the jobs. The millis() function passes the current system time to the run function of the job class which then decides if the job should be run.
To save power, i could put the loop into an timer controlled ISR, and let the cpu sleep, but power consumption is currently a secondary concern.

    while (1)
    	{
    		for ( auto p = Job::registry; p; p=p->chain )
    		    p->run(millis());
    	}

**Lambdas**

Lambdas are nice to avoid a callback function.

instead of 

    void handleserial(void)
    {
    	mqttrouteradapter.handleSerialMQTT();
    }    
    Job jserial(handleserial,10);

i can write something like

    Job jserial([](){mqttrouteradapter.handleSerialMQTT();},10);


**the new meaning of auto** 

Auto allows to deduce the type automagically. This function iterates over "something" passed the second parameter. It compiles, if the passed type provides the operations/attributes used in the function.
This allows separation of concerns, generic programming - i just like it.
The feature that the [template deduction mechanisms work also for function parameters][4] has been added in c++17.
A good reason to upgrade the toolchain :)

	void runRx(const char* data, auto msg_registry) {
		for (auto p = msg_registry; p; p = p->chain) {
			p->rx(data);
		}
	}

**using a recent compiler**

If you want to use a recent compiler (The Arduino IDE uses gcc4.9) you can [build your own gcc avr toolchain rather easily][3]. Then you have gcc 7.2 with c++17 support !

My conclusion is, there is no excuse using c, except you havn't yet learned c++. 

  [1]: http://www.avrfreaks.net/forum/c-new-delete-operator-confusion
  [2]: https://haarer.github.io/c++/avr/templates/i/o/2018/01/21/c++-template-based-hardware-access-vs-classic-approach.html
  [3]: https://github.com/haarer/toolchain68k
  [4]: http://en.cppreference.com/w/cpp/language/auto
  [5]: https://arduino.stackexchange.com/questions/49098/how-can-arduinos-8-bit-microcontrollers-handle-c/49113#49113
