# Movie Script Analysis using Hadoop MapReduce

## Project Overview
This project aims to analyze a movie script by processing the dialogue data using Hadoop MapReduce. It focuses on three main tasks:
1. **Most Frequent Words by Character**
2. **Dialogue Length Analysis**
3. **Unique Words by Character**

The analysis is performed using Hadoop's MapReduce framework to efficiently process large amounts of script data and generate insights on the most common words used by characters, the length of their dialogues, and the uniqueness of the words they use.

## Approach and Implementation
### Mapper and Reducer Logic

#### Task 1: Most Frequent Words by Character
- **Mapper**: 
  - Reads each line from the input file, splits the dialogue into words, and emits the word and character name.
- **Reducer**: 
  - Groups words by character and counts the occurrences of each word for a specific character.

#### Task 2: Dialogue Length Analysis
- **Mapper**: 
  - Reads each line and counts the number of words in the dialogue of a specific character.
- **Reducer**: 
  - Aggregates the word count for each character, resulting in the total length of their dialogues.

#### Task 3: Unique Words by Character
- **Mapper**: 
  - Tokenizes each dialogue and emits unique words per character.
- **Reducer**: 
  - Aggregates unique words for each character.

### Execution Steps
1. **Set up Hadoop and Docker containers**:
   - Run Hadoop in Docker containers (namenode, datanode, resourcemanager, etc.).
   - Ensure HDFS is properly configured and running.

2. **Compile the Java Project**:
   - Run `mvn clean package` to compile the project and generate the JAR file.
   
3. **Load the Input File into HDFS**:
   - Copy the script file (e.g., `movie_dialogues.txt`) into HDFS using the following command:
     ```bash
     hdfs dfs -put /movie_dialogues.txt /user/hadoop/movie_script/
     ```

4. **Run the MapReduce Job**:
   - Execute the Hadoop MapReduce job with the following command:
     ```bash
     hadoop jar /hands-on2-movie-script-analysis-1.0-SNAPSHOT.jar com.movie.script.analysis.MovieScriptAnalysis /user/hadoop/movie_script/movie_dialogues.txt /user/hadoop/output
     ```

5. **Retrieve Output from HDFS**:
   - Use the following command to get the output from HDFS:
     ```bash
     hdfs dfs -get /user/hadoop/output/task1 /local/directory
     ```

6. **View the Results**:
   - After executing the above commands, view the result files in the output directories (`task1`, `task2`, `task3`).

### Challenges Faced & Solutions
- **Challenge 1**: Difficulty in transferring files from local machine to the Hadoop container.
  - **Solution**: We used the `docker cp` command to copy the required files from the local file system to the container, and then loaded them into HDFS.
  
- **Challenge 2**: Inconsistent results when accessing the output.
  - **Solution**: We ensured the output directories were correctly created in HDFS and used proper file paths during the `hdfs dfs -get` operation.

### Sample Input and Output
INPUT:
Harry: I am the one who will defeat you.
Hermione: Books! And cleverness!
Ron: I'm not afraid of the dark.

OUTPUT:
Harry_I 1  
Harry_am 1  
Harry_the 1  
Harry_one 1  
Harry_who 1  
Harry_will 1  
Harry_defeat 1  

Hermione_Books 1  
Hermione_and 1  
Hermione_cleverness 1  

Ron_I 1  
Ron_not 1  
Ron_afraid 1  
Ron_dark 1  

