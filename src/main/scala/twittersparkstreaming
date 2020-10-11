import java.io._
import java.util
import java util.Date
import org.apache.hadoop.until.progressable
import org.apache.hadoop.fs
import org.apache.hadoop.fs.FSDataInputStream
import twitter4j.{RateLimieStatus,Responselist,URLEntity,User}
//import scala.collection.JavaConversions._
import javax.security.auth.login.Configuration
import org.apache.spark.{SparkConf,SparkContext,status,_}
import twitter4j.{Ouery,Status,Twitter,TwitterFactory}
import org.apache.spark.sql.functions_
import org.apache.spark.rdd.RDD
import org.apache.spark.streaming.twitter.TwitterUtils
import org.apache.spark.streaming.{Seconds,SteamingContext}
import twitter4j.Status
import twitter4j.conf.ConfigurationBuilder
import twitter4j.auth.OAuthAuthorization
import org.apache.hadoop.conf.Configuration
import org.apache.hadoop.fs.{FSDataOutputStream,FileSystem,FileUtil,Path}
import org.apache.spark.streaming.api.java.JavaReceiverInputDStream
import org.apache.spark.streaming.twitter.TwitterUtils
import scala.collection.JavaConverters._
import org.apache.hadoop.fs.FileSystem
import java.io.IOException
import java.util.logging.FileHandler
import java.util.logging.Logger
import java.util.logging.Level
import au.com.bytecode.opencsv.CSVWriter
import org.apache.hadoop.conf
import org.apache.spark.sql.{DataFrame,SQLContext}

import scala.collection.mutable.ListBuffer

//import org.apache.spark.sql.SQLContext.implicits._

import org.apache.spark.sql.hive.HiveContext
@throws[IOException]
object twittersparkstreaming {
  //Logger class object is created for implementing logging
  private val LOGGER=Logger.getLogger(twittersparkstreaming.getClass.getName)
  def main(args:Array[String]) {
      //creating entry point for the spark app
	  val sparkConf = new SparkConf().setMaster("local[1]").setAppName("Streaming Twitter")
	  val sc = new SparkContext(sparkConf)
	  LOGGER.log(Level.INFO."Spark configuration is created") 
    //Twitter Authentication credentials
System.setProperty("twitter4j.oauth.consumerKey", "9HWI95JHLtKfrtsli8c8hQBx")
System.setProperty("twitter4j.oauth.consumerSecret","6rq04n4mxFY1QRGN6Gxo1UvRv0t4Ws01hvJNeEh9IXZOMSm0o")
System.setProperty("twitter4j.oauth.accessToken", "3650362332-D6yKZiqCwGz4PgN62vsqZtrxo3rh1QJ0AuBS222")
System.setPrpoerty("twitter4j.oauth.accessTokenSecret","lxQ0CucV2UNovuyD55BCfmm1EmJqHkZiCx4jitzsQqGEc")

LOGGER.log(level.INFO, "Twitter configurations set as properties")

//val stream = TwitterUtils.createStream(streamingContext,None).window(Seconds(4))
val sqlContext = new SQLContext(sc)
import sqlContext.implicits._
//setting hadoop file system configuration
val hadoopConfig = new conf.Configuration()
hadoopConfig.addResource(new Path("/etc/hadoop/conf/core-site.xml"));
hadoopConfig.addResource(new Path("/etc/hadoop/conf/hdfs-site.xml"));
//hadoopConfig.set("fs.defaultFS", "hdfs://lxhdpmastdev001.lowes.com:50470/")
val fs = FileSystem.get(hadoopConfig)
val file = new Path("/hdptmp/s0990sjn/twitter1.csv")
LOGGER.log(level.INFO, "File System set to hdfs")
//if the file doesn't exist creating a new file 
if (!fs.exists(file)) {
import org.apache.hadoop.util.Progressable
val os = fs.create(file,new Progressable() {
  override def progress(): Unit = {
    //out.println("...bytes written: [ " + bytesWritten + " ]")
 }
})
//An object of csvwriter class is created to write to csv file
val outputFile = new BufferedWriter(new OutputStreamWriter(os, "UTF-8"))
val csvwriter = new CSVWriter(outputFile)
csvWriter.writeNext(Array("Name", "Tweet", "Language"))
//An instance of the twitter class is created using TwitterFactory
val twitter = new TwitterFactory().getInstance
//Getting the timeline if user

val statuses = twitter.getUserTimeline
LOGGER.log(level.INFO, "UserTimeline is obtained in statuses")
//Iterating through the list of statuses 
val it1 = statuses.iterator()
while (it1.hasNext) {
val status = it1.next
//Writing each tweet information as a row to csv file 
csvWriter.writeNext(Array(status.getUser.getName,status.getText,status.getLang))
       }
	   
	  //csvWriter.writeAll(listofRecords.toList)
	  outputFile.close()
	  LOGGER.log(Level.INFO, "File is closed" )
	  fs,close()
	  LOGGER.log(Level.INFO, "HDFS is closed")
	}
	  //else appending to an existing file
	  else {
	    import org.apache.hadoop.util.Progresable
		fs.open(file)
		//An object of csvwriter class is created to write to csv file
		val outputFile = new BufferedWriter(new OutputStreamWriter(new OutputStreamWriter(os, "UTF-8"))
		val csvWriter = new CSVWriter(outputFile)
		//An instance of the twitter class is created using TwitterFactory 
		val twitter = new TwitterFactory().getInstance
		
		//Getting the timeline of user
		
		  val statuses = twitter.getUserTimeline
		  LOGGER.log(Level.INFO, "UserTimelione is obtained in statuses")
		  //Interating through the list of statuses
		  val it1 = statuses.iterator()
		  while (it1.hasNext) {
		     val status = it1.next
			 //Writing each tweet information as a row to csv file
			 csvWriter.writeNext(Array(status.getUser.getName,status.getText,status.getLang))
			 }
			 
		  //csvwriter.writeAll(listofRecords.toList)
          outputFile.close()
		  LOGGER,log(Level.INFO, "File is closed")
		  fs.close()
		  LOGGER.log(Level.INFO, "HDFS is closed")
		 }
		 
		

		}
	}	
