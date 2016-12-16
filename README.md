# TJudger ( NowVersion 1.0)
This is a Judger for onlinejudge and local test.  
#Usage
##For Python:
###Normal Edition:
**Run ``sudo python setup.py install``**  
It will help you install python-dev and libseccomp-dev which is in need for judger.  
Import module:  
``import TJudger``  
There should be a dict to describe config.  
For instance:

	CFG = {
		'language':'C++', # compile language
		'source_name':'lll.cpp', # source file name
		'in_file':'lll.in', # input file name
		'out_file':'lll.out', # output file name
		'ans_file':'lll.ans', # answer file name
		'time_limit':1000, # time limit(ms)
		'memory_limit':64, # memory limit(MB)
		'compile option':['-O2', '-lm', '-DONLINE_JUDGE'] # compile options
		['special_judge':'spj.cpp', # special judger name]
		['spj_language':'C++' # special judger language]
	}
Then you will get result from ``RES = TJudger.run(CFG)``.  
The result is also a dict:

	RES = {
		'status': 'Wrong Answer', # run status
		'compile_info': '', # if compile fail, it will show compile error message. Otherwise, it's empty.
		'use_memory': 7728L, # the total memory program used
		'in': '2 3\n', # input detail
		'use_time': 0L, # the user time program used
		'score': 0L, # the score program can get(top: 100)
		'ans': '5  ', # answer detail
		'out': '' # output detail
	}

For details, check ``test/test.py`` and ``test/test_spj.py``.  
###Exact Edition:
Because of ``fork()`` may lead to unreliable memory using in python when actual memory using is very small, using ``os.system("./runner")`` to run judger in a new program in Exact Edition does do better in memory using detection instead of running in same program by API.  
In order to use Exact Edition:  
**Run ``sudo python Exsetup.py install``**  
**Then copy ``runner`` which is be compiled after Run ``sudo python Exsetup.py install`` to your judger root directory**  
Import module:  
**``import ExTJudger`` insead of ``import TJudger``**  
You can get result from ``RES = ExTJudger.run(CFG) `` instead.  
The other thing is the same as Normal Edition.  
For details, check ``test/Extest.py`` and ``test/Extest_spj.py``( there should also be a ``runner`` ) .
##For C/C++:
###Normal Edition:
**Check if you have installed ``libseccomp-dev``**  
If not, you need to install it with``sudo apt-get install libseccomp-dev``.  
**You should compile your juger source with ``-lseccomp``**  
Header:  
``#include "xxxx/src/judger.c"``
Then initialize Config and Result with:

	char* argv[] = {"xxx", "xxx", ...};
	Config CFG = {
		language, soure file name, in file name, out filename, ans file name, 
		time limit(ms), memery limit(MiB), argv[, special judger name][, special judger language]};
	Result RES;
Then run:  
``RES = run(&CFG)``  
The result is a struct defined in ``src/judger.h``, you can view all things in RES by:

	RES.score // the score program can get(top: 100)
	RES.compile_info // if compile fail, it will show compile error message. Otherwise, it's empty.
	RES.status // run status
	RES.use_time // the user time program used
	RES.use_memory // the total memory program used
	RES.in // input detail
	RES.out // output detail
	RES.ans // answer detail
 
For details, check ``test/test.c``.  
###Exact Edition:
**You should compile your juger source with ``-lseccomp`` and ``-DEXACT_MOD``**  
#About Special Judge:
The code in ``[]`` is in need for the judgement with special judger. And if you dont have special judger, you can ignore this option and leave it blank.  
Special judger also runs in sandbox and have time/memory limit for system safety. You can change this option in ``src/judger.c``.  
Your special judger should code with ``int main(int argc, char *argv[])``. Because main judger pass the message (in file content, out file content, ans file content) to special judger by argv in order (in = argv[0], out = argv[1], ans = argv[2]).  
And your special judger should print the score that program can get in stdout. The score needs to be in the range of 0 ~ 100. Otherwise it may lead to judger error. Main judger will and only will read 8 characters in your special judger output and cut out by last num can get.
#Status details
	System Error: Config is not correct
	Judger Error: Judge system goes wrongly
	Accepted: Run successfully and get the correct output
	Wrong Answer: Run successfully but get the wrong output
	Dangerous System Call: The program is killed because of runing with dangerous system call
	Runing Error: The program is killed because of error like segmentfault
	Compile Error: Your program sourse cannot be compiled correctly
	Time Limit Exceed: The Time your program costs is too large
	Memory Limit Exceed: The memory your program costs is too large
	Output Limit Exceed: The output of your program is too large
	Run Successfully: Run successfully but something goes wrong....
	with spj:
	Compile Special Judge Error: Your special judge source cannot be compiled correctly
	Run Special Judge Error: Your speical judger gose wrongly
	Partly Correct: Special judger find that your answer is not completely wrong
	

( Special judge mod is untested. It will be coming soon )  
**( Normal Edition memory using is based on ru_minflt. That may lead to the memory using which is detected is bigger than which is actually used )**  
**( Exact Edition memory using is based on ru_maxrss )**


##[Development Records](http://xtt.lcybox.com/wp-content/themes/XTT_A/article_tmp/sandbox.php)

