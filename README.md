# warp10-udf-riakts
This project correspond to a [Warp 10 UDF](http://www.warp10.io/reference/miscellaneous/UDF/#sidebar) to load GTS from Riak-ts tables.

## Usage
This function takes 4 parameters as entry: a riak url, a riak port, a column map and a riak query.
The column map correspond to the column from the result table generated by the query. Timestamp and class field are mandatory. 
For each key, associate the position in the table (Starting from 0). 
For the class and the labels keys, the value correspond to a map with the position as key, and the wanted name in Warp10 as value.

Here a small example is given. As there is no escape in WarpScript, to put a "'" the hexadecimal code "%27" is used.
```
'127.0.0.1' // riak URL 
8087        // riak Port
            // A map
{
    'timestamp' 2
    'class' 
    {
        3 'class1'
        4 'class2'
    }
    'labels'
    {
        0 'status'
        1 'family'
    }
    'elevation' 5
    'latitude'  6
    'longitude' 7
}
            // A query
'select *  from table where ts > 1 and ts < 3 and status=%27OK%27'
'io.warp10.riakts.LOADFROMRIAK' UDF
```


## Set-up 
Build this project, and then add the generated jar in Warp 10 server.

### Build the Jar ###
Clone this repository then execute the following command to build a jar.
```
gradle shadowJar
```

### Warp 10 conf
Open the Warp 10 conf file where you have to configure the following parameters to add a directory containing your udf on the Warp10 platform. Then copy the jar generated by gradle in warpscript.jars.directory.
```
warpscript.jars.directory = /some/dir/udf
warpscript.jars.refresh = 60000
```
