# Questions

## HPC good citizenship

1. On the UCI cluster, the resource request "-pe openmp 64" refers to the number of processors requested.  Does that
   request mean that your commands will use multiple processors?
   
   **Response:** No, because not all programs use multiple processors. 
   
2. In general, how do you know how many processors, how much RAM, how many files would be required/needed/written by thejobs you are running on the cluster?

  **Response:** To know how many processors, RAM, and how many files would be required/needed/written by the jobs I am running on the cluster, I would run on a subset of my data ```/usr/bin/time -v "parameters of program"``` The output will tell me the percentage CPU, maximum resident set size in kbytes, and output size of the job. From there, I can calculate the amount of processors, RAM, and files will be needed for the job. 

3. In order to be a "good citizen", you need to have some idea of much RAM your job requires.  In particular, you need
   to know the "peak" (i.e., maximum) RAM required at any point during execution.  Show an example of the shell commandthat you would use on a Linux machine to measure run time and peak ram usage of an arbitrary command, where the time/peak RAM values are written to a file.
   
**Response:** To see how much RAM and time my job requires, I first change my script to a command using ```"chmod +X midas_volatiles.sh"``` with midas_volatiles.sh being the name of my script. Then I run:  ```"/usr/bin/time -v bash midas_volatiles.sh 2>&1 | grep -e "User time (seconds)" -e "Maximum resident set size (kbytes):" > test.txt"``` to write the time/peak RAM and time the job will require to a file. 
   
4. What are the units of your answer for number 3?

**Response:** The units for user time is in seconds and the units for the maximum resident set size or RAM is in kbytes.

5. What are the bash commands for the following operations:

    * Checking that a file exists
    * Checking that a file exists and is not empty
    
  **Response:** To check that a file exists, use : ```test -f test.txt && echo "Exists" || echo "does not exist"``` . If the file exists, it the response will be Exists. If it does not exist, it will print does not exist. To check that a file exists and is not empty, use: ```test -s test1.txt && echo "exists and > 0" || echo "exists but empty"```

6. How would you use the commands from your answer to 5 to write a work flow for HPC that only runs a job if the
   expected output file is **not** present.

**Response:** To run a job if the expected outfile is not present, I would write an if and then statement such as: 
  ```"if [!-e "file_name"]; then"``` and then run the lines of code for the job. 
  

## Trickier questions

7. Would your answer to number 3 work on Apple's OS X operating system?  If no, do you have any idea why not? 

**Response:** Using /usr/bin/time -v on a Mac OS, the response is that the option --v is illegal. However, there is an command called time which can be run. To have the same function as what is on the HPC, I have to install homebrew and then install the gnu-time package. After the installations, there is a gtime command that will act in the same way. 

8. Most of the HPC nodes have 512Gb (gigabytes) of RAM. Let's say you have a job that will require **no more** than 24Gb
   of RAM.  How would you request resources so that you can run more than one job on a node **and** not cause nodes to
   crash?  Show an example of a skeleton HPC script as part of your answer.  **Knowing how to do this is super important
   and will save you loads of frustration and prevent you from taking out your colleagues' jobs on the cluster,
   preventing you from getting nasty emails from Harry!!!!!!!!!!!**
   
**Response:** To run more than one job and a node and not cause nodes to crash, you can create an array job. 
```
#!/bin/bash

#$ -q bio,pub64
#$ -tc 128
#$ -N test_name
#$ -S /bin/bash
#$ -j y 
#$ -t 1-1000
#$ -pe openmp 24
#$ -l mem_size=2G
```
