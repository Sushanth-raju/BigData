package sush1;

import java.io.IOException;          // importing java input output statements

import java.util.StringTokenizer; 	// breaks the string into tokens 

 

import org.apache.hadoop.conf.Configuration;  // configuration of system parameters

import org.apache.hadoop.fs.Path; 	//  distributed implementation of filesystem for reading and writing	

import org.apache.hadoop.io.IntWritable; //compares the intwritables

import org.apache.hadoop.io.Text; //stores text

import org.apache.hadoop.mapreduce.Job; 

import org.apache.hadoop.mapreduce.Mapper; 

import org.apache.hadoop.mapreduce.Reducer; 

import org.apache.hadoop.mapreduce.lib.input.FileInputFormat; 

import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat; 


 

public class wordcount { 

 

  public static class TokenizerMapper 

       extends Mapper<Object, Text, Text, IntWritable>{ 

 

    private final static IntWritable one = new IntWritable(1);  // creating an object called one as private

    private Text word = new Text(); 	//creating an object called word as private

 

    public void map(Object key, Text value, Context context 

                    ) throws IOException, InterruptedException { 

      StringTokenizer itr = new StringTokenizer(value.toString()); 		//type conversion 

      while (itr.hasMoreTokens()) { 

        word.set(itr.nextToken()); 

        context.write(word, one); 

      } 

    } 

  } 

 

  public static class IntSumReducer 

       extends Reducer<Text,IntWritable,Text,IntWritable> { 

    private IntWritable result = new IntWritable(); 

 

    public void reduce(Text key, Iterable<IntWritable> values, 

                       Context context 

                       ) throws IOException, InterruptedException { 

    	// creating a method reduce called once for each key and using throw keyword , incase if it has any errors , 
    	// throw block captures it
    	
      int sum = 0; 							  //Initializing sum = 0

      for (IntWritable val : values) { 

        sum += val.get(); 

      } 

      result.set(sum);  						// setting the result on to the sum
      
      context.write(key, result); 				// displaying the result stored in it

    } 

  } 

 

  public static void main(String[] args) throws Exception {    // here set method works until the work is done
	  														   //otherwise  it throws an exception
    Configuration conf = new Configuration(); 

    Job job = Job.getInstance(conf, "word count"); 			// used to return the object by implementing the logic

    job.setJarByClass(wordcount.class);                		// tells the nodes where to look for Mapper & Reducer classes.
	

    job.setMapperClass(TokenizerMapper.class); 		       // used to set mapper class to the driver class

    job.setCombinerClass(IntSumReducer.class); 				// it sets the combiner for job

    job.setReducerClass(IntSumReducer.class); 				// sets the reducer for the job

    job.setOutputKeyClass(Text.class); 

    job.setOutputValueClass(IntWritable.class); 

    FileInputFormat.addInputPath(job, new Path(args[0])); 

    FileOutputFormat.setOutputPath(job, new Path(args[1])); 

    System.exit(job.waitForCompletion(true) ? 0 : 1); 			// if the job is successfully executed it will return 
    														    // true and exit the system			

  } 

} 

 
