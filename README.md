google-go-monte-carlo
=====================

Code written in Google Go as a way to "get my feet wet" with the language.


Performance
===========

All tests were done on my iMac with an Intel i7 CPU running at 3.4 Ghz.
It has 4 physical cores and each core has 2 virtual cores on it.  
Since Go saw the number of CPUs as 8, that's the setting I used.
Instead of the actual random number generator, I used my MD5 faux 
random number generator. This helped me separate user CPU load from 
system CPU load (caused by messages being sent), which was most 
helpful to me in troubleshooting.  I used version 1.0.3 of Google Go.

Originally, many channels were created (it helped me learn them in Go...), 
but since the time to create a single random number is so low, the overhead 
of several messages sent across each channel for each random number 
generated was a problem.  Here is a list of the the changes I made and
their improvements when running `go run ./main.go --size 100 --num-points 1000000`:

- Original method: 13 seconds
- Modified random number generation to use a chunk size of 10K: 9.7 seconds
- Merged getPoints() and checkPoints(), eliminating 1 million writes 
	to a channel: 7.7 seconds
- Changed Pi calculation to be at end of run without using messages, 
	saved another 1 million writes to a channel: 4.3 seconds
- Modified intNChannel() to return arrays of 2 random numbers instead 
	of a random number at a time, saved yet another 1 million channel writes: 2.5 seconds



