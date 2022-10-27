---
layout: post 
title: "Find Command"
permalink: /labs/lab-report-3-week-5
---
# Lab Report 3 (Week 5)
In this lab report you will be introduced to the find command and some specific use cases.

## What is find?
Find is a command that recursively searches a directory or a specified portion of a directory for a file.
The syntax for a basic use of the command looks like this:
`find <start_directory>`
This will return all the files **and** directories starting from start_directory.
If you want to search for a specific file name you would use:
`find <start_directory> -name "<expression>"`
This would return only the file and directory names that match with the given expression.

Here is an example below:
```
$ find technical
technical
technical/government
technical/government/Media
technical/government/Media/Terrorist_Attack.txt
technical/government/Media/Domestic_violence_aid.txt
technical/government/Media/fight_domestic_abuse.txt
technical/government/Media/Entities_Merge.txt
technical/government/Media/Free_legal_service.txt
technical/government/Media/Philly_Lawyers.txt
technical/government/Media/Making_a_case.txt
...
```
This command used find to list all the files and directories found under the directory called technical. The output was truncated due to length.
Now let's look at some interesting options that make this tool more powerful.

## Find with prune!
Imagine you want to search for a specific file in a directory with an insane amount of branches! You know that the file is not in a specific branch. If only there was a way to ignore that specific branch...
Find has got you covered with an option called prune!
Prune allows you to leave out a certain path when printing your results. Let's take a look at some examples below.
```
find <start_directory> -path <path_name> -prune -o [options]
```
What command this does is recursively searches for files starting at <start_directory>. If it finds the path <path_name>, it will prune it, otherwise it will execute the given [options]. Pruning will prevent the path from being searched on any further, excluding it from the output. The [options] will specify any other options you want to add, such as `-name <expression>` to search for a specific file name. Let's look for some examples

1. find -prune with -print
  ```
  $ find technical -path technical/plos -prune -o -print
  technical
  technical/government
  technical/government/Media
  technical/government/Media/Terrorist_Attack.txt
  technical/government/Media/Domestic_violence_aid.txt
  technical/government/Media/fight_domestic_abuse.txt
  technical/government/Media/Entities_Merge.txt
  technical/government/Media/Free_legal_service.txt
  technical/government/Media/Philly_Lawyers.txt
  technical/government/Media/Making_a_case.txt
  ...
  technical/government
  technical/government/Media
  technical/government/Media/Terrorist_Attack.txt
  technical/government/Media/Domestic_violence_aid.txt
  technical/government/Media/fight_domestic_abuse.txt
  technical/government/Media/Entities_Merge.txt
  technical/government/Media/Free_legal_service.txt
  technical/government/Media/Philly_Lawyers.txt
  technical/government/Media/Making_a_case.txt
  technical/government/Media/Legal_system_fails_poor.txt
  ...
  technical/911report/chapter-13.5.txt
  technical/911report/chapter-3.txt
  technical/911report/chapter-2.txt
  technical/911report/chapter-1.txt
  technical/911report/chapter-13.3.txt
  technical/911report/chapter-13.4.txt
  technical/911report/chapter-11.txt
  technical/911report/chapter-13.1.txt
  technical/911report/chapter-12.txt
  technical/911report/chapter-5.txt
  ```
  As you can see, this command printed all files and  directories found under the technical directory, but did not   include files in the plos directory.

2. find -prune with -name
  ```
  $ find technical -path technical/plos -prune -o -name "[0-9]  *"
  technical/government/Media/5_Legal_Groups.txt
  technical/government/Env_Prot_Agen/1-3_meth_901.txt
  technical/biomed/1472-6904-2-5.txt
  technical/biomed/1472-6947-2-7.txt
  technical/biomed/1471-2296-3-18.txt
  technical/biomed/1471-2164-4-2.txt
  technical/biomed/1472-6769-1-2.txt
  technical/biomed/1471-2164-2-6.txt
  technical/biomed/1472-6793-2-5.txt
  technical/biomed/1471-2202-2-15.txt
  ...
  technical/biomed/1471-2121-3-30.txt
  technical/biomed/1471-2121-2-18.txt
  technical/biomed/1471-2202-3-4.txt
  technical/biomed/1471-2091-3-4.txt
  technical/biomed/1471-2105-3-14.txt
  technical/biomed/1471-2105-3-28.txt
  technical/biomed/1472-6947-3-5.txt
  technical/biomed/1471-2105-4-26.txt
  technical/911report
  technical/plos
  ```
  As you can see this command searched for all files with names that start with a number followed by other characters. However files in the plos branch was omitted. There are no files in the 911report directory that start with a number so nothing was printed there either.

3. 

  ```
  $ find technical -path "technical/[a-z]*" -prune -o -print
  technical
  technical/911report
  technical/911report/chapter-10.txt
  technical/911report/preface.txt
  technical/911report/chapter-6.txt
  technical/911report/chapter-9.txt
  technical/911report/chapter-13.2.txt
  technical/911report/chapter-7.txt
  technical/911report/chapter-8.txt
  technical/911report/chapter-13.5.txt
  technical/911report/chapter-3.txt
  technical/911report/chapter-2.txt
  technical/911report/chapter-1.txt
  technical/911report/chapter-13.3.txt
  technical/911report/chapter-13.4.txt
  technical/911report/chapter-11.txt
  technical/911report/chapter-13.1.txt
  technical/911report/chapter-12.txt
  technical/911report/chapter-5.txt
  ```

  This command finds all files in the technical directory but with all directories that start with a lower case letter pruned out. The result is that only files in the 911report directory is output.


## Find with exec!
The -exec option allows you to execute a command on every file that is discovered by find. The syntax for this option is as follows:

`find <start_dir> -name <expression> -exec [commands] {} \;`

1. 

  ```
  $ find technical -name "*.txt" -exec grep -l "RDBMS" {} \;
  technical/biomed/1471-2105-3-2.txt
  ```
  As you can see this command used find to get all files with a .txt extension. Then the -exec flag is used to execute a command, in this case grep. The {} represents each file found by find, and the \; represents the end of the command. This specific example uses find -exec and grep to find all txt files that contain the phrase RDBMS. Only one file does.

2. 

  ```
  $ find technical -name "*.txt" -exec wc {} \;
    254  2421 14232 technical/government/Media/ Terrorist_Attack.txt
    84  626 3765 technical/government/Media/  Domestic_violence_aid.txt
    58  450 2806 technical/government/Media/  fight_domestic_abuse.txt
    88  744 4366 technical/government/Media/Entities_Merge.txt
    60  443 2702 technical/government/Media/Free_legal_service. txt
    87  688 4263 technical/government/Media/Philly_Lawyers.txt
   118  967 5847 technical/government/Media/Making_a_case.txt
   135 1251 7218 technical/government/Media/  Legal_system_fails_poor.txt
    91  732 4540 technical/government/Media/Barnes_new_job.txt
    75  607 3698 technical/government/Media/defend_yourself.txt
    ...
  ```
  This example calls wc on all txt files found in the technical directory.

3.
  ```
  $ find technical -name "*.txt" -exec echo "test: {}" \;
  test: technical/government/Media/Terrorist_Attack.txt
  test: technical/government/Media/Domestic_violence_aid.txt
  test: technical/government/Media/fight_domestic_abuse.txt
  test: technical/government/Media/Entities_Merge.txt
  test: technical/government/Media/Free_legal_service.txt
  test: technical/government/Media/Philly_Lawyers.txt
  test: technical/government/Media/Making_a_case.txt
  test: technical/government/Media/Legal_system_fails_poor.txt
  test: technical/government/Media/Barnes_new_job.txt
  test: technical/government/Media/defend_yourself.txt
  test: technical/government/Media/Wingates_winds.txt
  test: technical/government/Media/City_Council_Budget.txt
  ...
  test: technical/plos/pmed.0020268.txt
  test: technical/plos/pmed.0020181.txt
  test: technical/plos/journal.pbio.0030131.txt
  test: technical/plos/journal.pbio.0020105.txt
  test: technical/plos/pmed.0020086.txt
  test: technical/plos/pmed.0010022.txt
  test: technical/plos/pmed.0020235.txt
  test: technical/plos/pmed.0020191.txt
  test: technical/plos/pmed.0020242.txt
  ```

  This command combined the find -exec option with the echo command. This command echoed all the files ending with .txt in the technical directory with "test: " appended to the output.

## Find with min!

The min option allows you to find files that were accessed, changed, or modified in the last specified minutes. Files that were modified had its contents altered, or it was created. On the other hand, changed files may have been somehow altered without having its contents changed. For example, permissions changed, or its location moved. Finally, if a file has been accessed, it may have been read without having changed any other feature. 

There are 3 ways to specify a time interval: `-n +n n`

1. -n refers to everything in the last n minutes.

2. +n refers to all after the n minutes

3. n refers to all at exactly the n minute

The following syntaxes apply:
`find <start_dir> -name <expression> -amin -n`

1. find -n
  ```
  $ find technical -name "*.txt" -cmin -480
  technical/plos
  technical/plos/1a.txt
  ```

  For this example, the file 1a.txt was created about 7 hours ago at the time of execution. This command searched in the directory technical ending with .txt that was created in the last 8 hours. The command found the file 1a.txt and the directory technical/plos because they were changed in the last 8 hours. 

2. find +n

  ```
  $ find technical -name "*.txt" -cmin +1
  technical/government/Media/Terrorist_Attack.txt
  technical/government/Media/Domestic_violence_aid.txt
  technical/government/Media/fight_domestic_abuse.txt
  technical/government/Media/Entities_Merge.txt
  technical/government/Media/Free_legal_service.txt
  technical/government/Media/Philly_Lawyers.txt
  technical/government/Media/Making_a_case.txt
  technical/government/Media/Legal_system_fails_poor.txt
  technical/government/Media/Barnes_new_job.txt
  technical/government/Media/defend_yourself.txt
  ...
  technical/plos/journal.pbio.0030131.txt
  technical/plos/journal.pbio.0020105.txt
  technical/plos/pmed.0020086.txt
  technical/plos/pmed.0010022.txt
  technical/plos/pmed.0020235.txt
  technical/plos/pmed.0020191.txt
  technical/plos/pmed.0020242.txt
  ```

  This command searched for all txt files in the technical directory that was changed after the last 1 minute. This applies to practically all the files in the directory.

3. find n

  ```
  $ find technical -name "*.txt" -cmin 450
  technical/plos/1a.txt
  ```
  
  This command searches the technical directory for a file ending with .txt that was created exactly 450, or 7.5 hours ago. For this example it was `technical/plos/1a.txt`.


## View previous labs
[Lab Report 1](./lab-report-1-week-0.md)

[Lab Report 1.5](./lab-report-1-week-1.md)
