## wcs-rsync-tools
wcs-rsync-tool is a tool developed on basis of Object Storage API. It is used to synchronize local data to Object Storage with the original directory structure.
Note: wcs-rsync-hash is only applicable to the migration of stock files. For incremental files, please use Object Storage API.

### Applicable API Version
It's applicable for API-V1/SDK-V1.

### Download

#### Command Line Tool
[rsync-hash](https://www.wangsu.com/wos/draft/news/1638519119173/1638519119193_rsync-hash.zip)

#### Visual Tool
- windows:

[websync-windows-x86](https://www.wangsu.com/wos/draft/news/1638515760290/1638515760535_websync-windows-x86.zip)

[websync-windows-x64](https://www.wangsu.com/wos/draft/news/1638515911587/1638515911797_websync-windows-x64.zip)
- Linux:

[websync-linux-x86](https://www.wangsu.com/wos/draft/news/1638516578802/1638516579218_websync-linux-x86.zip)

[websync-linux-x64](https://www.wangsu.com/wos/draft/news/1638516578802/websync-linux-x64.zip)

Note: Download a tool correspongding to your OS.

### Configurations

|Parameters|Required|Description|
|--|--|--|
|accessKey|Yes|Available at [CDNetworks Console](https://dash.cdnetworks.com/) in “ Security Settings -> API information management -> AccessKey Management”|
|secretKey|Yes|Available at [CDNetworks Console](https://dash.cdnetworks.com/) in “ Security Settings -> API information management -> AccessKey Management”|
|uploadDomain|Yes|Domain name for uploading files, available at [CDNetworks Console](https://dash.cdnetworks.com/) in “Object Storage Service -> Buckets -> Overview -> Domain Names”|
|mgrDomain|Yes|The management domain name required for file HASH values comparison by the tool, available at [CDNetworks Console](https://dash.cdnetworks.com/) in “Object Storage Service -> Buckets -> Overview -> Domain Names”|
|syncMode|Yes|synchronization mode.Only supported by command line synchronization tool.Default at 0, supporting single-bucket and multi-directory upload mode, bucket and syncDir required, keyPrefix optional. If configured to 1, supporting multi-bucket and multi-file upload mode, bucketAndDir required.|
|bucket|No|If not null, files will be saved in bucket specified by this parameter. And Multi-bucket is not supported yet. Note that `syncMode` should be set at 0 when using the command line tool.|
|syncDir|No|Paths of source files. Multi-path is supported by separating paths with "\|", for example, "D:/pic-2\|D:/rsync3". Note that `syncMode` should be set at 0 when using the command line tool.|
|keyPrefix|No|A prefix specified by this parameter can be added to the file to upload , null by default. Multi-prefix is supported, which should be consistent with paths specified by `syncDir`.<br /> For example,<br /><1> If the file to upload is "1.apk" and keyPrefix of which is set at "data/", then the file should be saved as "data/1.apk" in Object Storage, that is, a new folder “data” is added, and “1.apk” is saved in the folder;<br /><2> If the file to upload is "1.apk" and keyPrefix of which is set at "data",then the file is saved as “data1.apk” in Object Storage;<br /><3> For multi-keyPrefix, syncDir is set at "D:/rsync1\|D:/rsync2\|D:/rsync3", keyPrefix is set at "test1/\|test2/", then files (folders) under rsync1 are saved under test1 derectory in bucket, files (folders) under Rsync2 are saved under test2 derectory, and files (folders) under rsync3 are saved in root directory. If more keyPrefix is configured than syncDir, the redundant keyPrefix does not take effect.<br />Please note that `syncMode` should be set at 0 when using the command line tool.|
|bucketAndDir|No|This field is to configure both your target bucket of Object storage and the local path where you want to sync files to Object Storage.<br />Only supported by command line tool, required when syncMode is set at 1. For example, bucket1\|D:/dir1, D:/dir2\|prefix1, prefix2/; bucket2\|D:/dir3, D:/dir4<br />Note:<br /><1> Each bucket can be configured with multiple local paths. Local paths support folders and files with suffixes;<br /><2> Local path corresponds to prefix, refer to keyPrefix for prefix use;<br /><3> Bucket name, local path and prefix are separated by "\|", multiple local paths or prefixes are separately by English comma ","<br /><4> Combination of multiple buckets, local paths and prefixes can be configured, separated by ";".<br />Note: Above example: bucket1\|D:/dir1,D:/dir2\|prefix1,prefix2/;bucket2\|D:/dir3,D:/dir4 means:<br />1. All files under your local path of D:/dir1 will be synced to bucket1 of Object Storage, and the file name is with the prefix of “prefix1”, for example, a.jpg will be synced as prefix1a.jpg.<br />2. All files under your local path of D:/dir2 will be synced to the directory of “prefix2” under bucket1 of Object Storage.<br />3. All files under your local path of D:/dir3 and D:/dir4 will be synced to the bucket2 of Object Storage.|
|threadNum|No|Number of threads to upload. Value range 1-100, 1 by default.|
|sliceThreshold|No|A size threshold. If file size is larger than the threshold, multipart upload will be used. Unit MB, range 1-100 MB, 4MB by default.|
|sliceThread|No|Number of threads for multipart upload. Value range 1-100, 5 by default.|
|sliceBlockSize|No|Size of block when multipart upload. 4MB by default. Range: 4MB-1024MB, the value should be a integer multiple of 4.|
|sliceChunkSize|No|Size of chunk when multipart upload. Range: 1024KB-1048576KB.<br />Note: `sliceThread`, `sliceBlockSize` and `sliceChunkSize` are only valid for multipart upload.|
|deletable|No|Deleting local file does not effect that in Object Storage when deletable is set at 0.<br />Deleting local file will do effect that in Object Storage when deletable is set at 1.<br />Note: The setting is only valid for files last update, invalid for other files historically synchronized. 0 by default.|
|maxRate|No|Limit to upload rate, unit KB/s. If it's set at 0, then no rate limit.|
|taskBeginTime|No|Upload begin time, format hh:mm:ss. For example 12:00:00.|
|taskEndTime|No|Upload end time, format hh:mm:ss. For example 15:00:00.|
|isCompareHash|No|Whether to compare hash when uploading.<br />Comparing hash if it's set at 0, otherwise not.|
|countHashThreadNum|No|Thead numbers to compute hash. Range 1-100, 1 by default. For example, if it's set at 10, then 10 threads are processed concurrently to compute hash. |
|compareHashThreadNum|No|Thead numbers to compare hash between loacal with Object Storage.Range 1-100, 1 by default. For example, if it's set at 10, then 10 threads are processed concurrently to compare hash.|
|compareHashFileNum|No|Number of file HASHes queried from Object Storage at a time when comparing file HASHes. Range 1-2000, 100 by default. For example, if it's set at 100, then HASHes of 100 files can be queried form Object Storage at a time.|
|minFileSize|No|The minimum size of a file to upload. 0 by default, which means no limit. For example, if it's set at 1024, then file size below 1024 byte are banned to upload.|
|overwrite|No|Whether or not to overwrite a file with the same name on Object Storage, the value can be 1 or 0. 1 indicates Overwrite, 0 indicates No Overwrite, 1 by default.|
|isLastModifyTime|No|whether to update lastModifiedTime of a file on Objcet Storage,  the value can be 0 or 1, 0 by default. 0 indicates to update, 1 indicates not.|
|scanOnly|No|If it's set at 1, the file list will be just read without computing or comparing Hash. Note: This is a risk parameter, please confirm with the Object Storage staff before set the parameter if it's required.|
|uploadErrorRetry|No|Times of automatic retry for failed uploads. Range 0-5, 0 by default. For example, if it's set at 2, it can be retried twice when the upload fail.|

### **Command Line Tool**
#### **Requirements**

1. Pre-install Java, JDK 1.6 or above
2. Put the config file and tool in same path

#### **How to use**
1. Open the directory where wcs-rsync-hash tool is located, such as Windows directory “F:\wcs-rsync” or Linux directory “/home/tool/wcs-rsync”
2. Configure conf.json
3. Start service

   In Windows, hold Shift in a blank area and right click, select “Open Command Window Here (W)”
Run the command: java -jar wcs-rsync-hash-xxx.jar conf.json
After the synchronization, run the command again to re-sync the files with failed synchronization.

4. List files with failed synchronization

   Run the command: java -jar wcs-rsync-hash-xxx.jar -listfailed conf.json. Output is saved to the log file in the tool directory

5. Force re-upload of all files: Run the command: java -jar wcs-rsync-hash-xxx.jar -igsync conf.json


### Visual synchronization tool

#### Window
1. Start the service

   Run startup.bat to pop up TOMCAT window
2. The first time the service is successfully opened, the browser will automatically open and enter the operation interface.

   After the service is successfully started, the TOMCAT window can be closed.
   
3. During service startup, remote operations can be performed using a browser by opening the browser and entering IP: Port to go to the operation interface

   Note: default Port at 8091*

4. Select “Configure Upload” to set basic information and advanced parameters as required

5. Click “Upload” to upload

   Stop the upload if configuration needs to be changed during uploading

6. View the upload progress through “Progress Query”, and failed files can be re-uploaded

7. Close the service

   Run shutdown.bat to close the service
#### Linux
1. Start the service

   Run startup.bat to pop up TOMCAT window
2. After service startup, remote operations can be performed using a browser by opening the browser and entering IP: Port to go to the operation interface.

   Note: default Port at 8091*

3. Select “Configure Upload” to set basic information and advanced parameters as required

4. Click “Upload” to upload

   Stop the upload if configuration needs to be changed during uploading

5. View the upload progress through “Progress Query”, and failed files can be re-uploaded

6. Close the service

   Run shutdown.bat to close the service

Note: Visual synchronization tools provide local configuration files… \service\wcsrSynchashWeb\conf.json, allowing users to customize whether configuration items are editable on the interface, “readonly: true” indicates that the configuration item is read-only. “readonly: false” indicates that the configuration item can be edited on the interface; configuration items that cannot be edited on the interface can be modified for their value in the configuration file.
```
If only accessKey, secretKey, bucket, syncDir and other basic configurations are excepted to be edited on the interface, set “readonly” to “false”, and “true” for those that are not expected to be edited on the interface
{
    "accessKey":{"value":"","readonly":false},
    "secretKey":{"value":"","readonly":false},
    "bucket":{"value":"","readonly":false},
    "syncDir":{"value":"","readonly":false},
    "uploadDomain":{"value":"","readonly":true},
    "mgrDomain":{"value":"","readonly":true},
    "keyPrefix":{"value":"","readonly":true},
    “threadNum":{"value":"1","readonly":true},
    ...

}
```
