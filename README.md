A terminal grading workflow. I wrote this to make grading lab reports less, you 
know, awful. 

This workflow pretty much eliminates the need to actually interact with the 
submission directory (i.e. endlessly opening and closing files, and navigating (more 
like orienteering) the submission directory's idiotic layout.) 

[Linux](https://archlinux.org) and [i3wm](https://i3wm.org/) centric.

<p float="left">
<br>
<img src="./media/student-selector.png" width="49%" />&nbsp;&nbsp; 
<img src="./media/comments-txt.png" width="49%" />
</p>

<p float="left">
<img src="./media/comments-pdf.png" width="49%" />&nbsp;&nbsp;  
<img src="./media/stats.png" width="49%" />
<br>
</p>

<br>

# CLI Flags

```
Usage: grade <dir> <option>

options:
	-c|--configure		configure directory for grading
	-g|--grade		grading comments selector
	-v|--view		student submissions selector
	-s|--save		save progress
	-p|--pdf		generate a pdf comments file for each student
	-t|--stats		show summary statistics
	-u|--upload		create upload directory
```

- `-c` to configure (i.e. initialize) the directory for grading. 
  + creates a `comments.txt` file in each student directory.
  + creates a `progress.csv` file in the grading directory, to log current 
  progress and prevent accidental overwrites. 
  + creates a `commentsbuffer.txt` file in the grading directory, which logs all 
  comments for the students graded so far (for easy access).  
- `-g` to open the grading comments selector. A list of students will be 
  printed, with ungraded ones highlighted in yellow ([Figure 
  1](https://github.com/knbe/grading/blob/main/media/student-selector.png)).  
  Enter the student's number to open their `comments.txt` file for grading. Both 
  the comments file and the commentsbuffer file are opened in Vim ([Figure 
  2](https://github.com/knbe/grading/blob/main/media/comments-txt.png)). 
- `-v` to open the student submissions selector. A list of students will be 
  printed, with ungraded ones highlighted in yellow. Enter the student's number 
  to open their submission attachment(s). Pdf files are opened with zathura, and 
  docx files are opened with LibreOffice. 
- `-s` to save current grading progress. This runs a script that parses the 
  `comments.txt` files for each student, saves their comments in the comments 
  buffer, saves their grades in a grades file, and writes the information to a 
  markdown file `comments.md`. 
  + N.B. the grades are saved in a file `grades-tmp.csv`, not the original 
  `grades.csv` file. This is so that a copy of the original grades file format 
  is preserved (in case anything weird happens). I've noticed that Classes is 
  rather fussy about the format of the grades file that gets uploaded (in 
  particular the variable names and the metadata at the top of the grades file.  
  If anything differs from the original download format, the grades don't get 
  uploaded correctly).  
- `-p` to generate a pdf comments file for each student. Uses 
  [Pandoc](https://pandoc.org/) to parse the `comments.md` files for each 
  student, and generates a `comments.pdf` file ([Figure 
  3](https://github.com/knbe/grading/blob/main/media/comments-pdf.png)). 
- `-t` to show summary statistics. Runs a script written in Julia that parses 
  the `grades-tmp.csv` file, and prints a grade histogram (using 
  [UnicodePlots.jl](https://github.com/Evizero/UnicodePlots.jl)) and some 
  summary statistics ([Figure 
  4](https://github.com/knbe/grading/blob/main/media/stats.png)). 
- `-u` to create an "upload" directory that contains only the necessary files 
  for uploading back to Classes. 

# Usage

- Download the assignment zip file from Classes. Tick the boxes for "Student 
  submission attachments", "Feedback attachments", and "grades.csv file". 
  Probably also tick the box for "include students who haven't submitted yet". 
- Unzip the directory, move to the desired location, etc. 
- Configure the grading directory by running `grade <dir> -c`. 
- Open the student selector by running `grade <dir> -g`. 
- Enter the "number" of the student you want to grade. This will open that 
  student's `comments.txt` file and the `commentsbuffer.txt` file (in Vim). 
- In a separate window, run `grade <dir> -v`. This will open the student 
  submissions selector, and you can open the student's submission attachments. 
- Once finished grading a particular student, save and close the `comments.txt` 
  file. Select the next student and repeat. 
- When finished grading all students, run `grade <dir> -u` to create an "upload" 
  directory. Zip this directory and upload to Classes. 
