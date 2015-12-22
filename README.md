# Proguard-Obfuscation-Retrace

# Enabling ProGuard (Gradle Builds)
Set the minifyEnabled property to true to enable ProGuard.

```Gradle
  android {
   ...
 
    buildTypes {
        release {
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android.txt'),
            'proguard-rules.pro'
        }
    }
  }
```

#Configuring ProGuard
For restoring a stack trace, you need to keep attributes (SourceFile and LineNumberTable)

```Proguard
-printmapping mapping.txt
-renamesourcefileattribute MyApplication
-keepattributes SourceFile,LineNumberTable
```

#Retrace
Assume that you got an obfuscated exception from Google Analytics like this.

```
java.io.IOException: Can't read [dummy.jar] (No such file or directory)
	at proguard.y.a(MyApplication:188)
	at proguard.y.a(MyApplication:158)
	at proguard.y.a(MyApplication:136)
	at proguard.y.a(MyApplication:66)
	at proguard.ProGuard.c(MyApplication:218)
	at proguard.ProGuard.a(MyApplication:82)
	at proguard.ProGuard.main(MyApplication:538)
Caused by: java.io.IOException: No such file or directory
	at proguard.d.q.a(MyApplication:50)
	at proguard.y.a(MyApplication:184)
	... 6 more
```	

Restoring a stack trace with line numbers

```
$java -jar retrace.jar mapping.txt stacktrace.txt
```

The output will correspond to the original stack trace:

```
java.io.IOException: Can't read [dummy.jar] (No such file or directory)
	at proguard.InputReader.readInput(InputReader.java:188)
	at proguard.InputReader.readInput(InputReader.java:158)
	at proguard.InputReader.readInput(InputReader.java:136)
	at proguard.InputReader.execute(InputReader.java:66)
	at proguard.ProGuard.readInput(ProGuard.java:218)
	at proguard.ProGuard.execute(ProGuard.java:82)
	at proguard.ProGuard.main(ProGuard.java:538)
Caused by: java.io.IOException: No such file or directory
	at proguard.io.DirectoryPump.pumpDataEntries(DirectoryPump.java:50)
	at proguard.InputReader.readInput(InputReader.java:184)
	... 6 more
```	

Restoring a stack trace with line numbers (verbose)

```
java -jar retrace.jar -verbose mapping.txt stacktrace.txt
```

The output will then look as follows:

```
java.io.IOException: Can't read [dummy.jar] (No such file or directory)
	at proguard.InputReader.void readInput(java.lang.String,proguard.ClassPathEntry,proguard.io.DataEntryReader)(InputReader.java:188)
	at proguard.InputReader.void readInput(java.lang.String,proguard.ClassPath,int,int,proguard.io.DataEntryReader)(InputReader.java:158)
	at proguard.InputReader.void readInput(java.lang.String,proguard.ClassPath,proguard.io.DataEntryReader)(InputReader.java:136)
	at proguard.InputReader.void execute(proguard.classfile.ClassPool,proguard.classfile.ClassPool)(InputReader.java:66)
	at proguard.ProGuard.void readInput()(ProGuard.java:218)
	at proguard.ProGuard.void execute()(ProGuard.java:82)
	at proguard.ProGuard.void main(java.lang.String[])(ProGuard.java:538)
Caused by: java.io.IOException: No such file or directory
	at proguard.io.DirectoryPump.void pumpDataEntries(proguard.io.DataEntryReader)(DirectoryPump.java:50)
	at proguard.InputReader.void readInput(java.lang.String,proguard.ClassPathEntry,proguard.io.DataEntryReader)(InputReader.java:184)
	... 6 more
```	

>
> **Note:** If you are not configue -keepattributes SourceFile,LineNumberTable to keep line number the output will list all alternative original method names for each ambiguous obfuscated method name like this.
>
```	
java.io.IOException: Can't read [dummy.jar] (No such file or directory)
	at proguard.InputReader.execute(InputReader.java)
	                        readInput(InputReader.java)
	at proguard.InputReader.execute(InputReader.java)
	                        readInput(InputReader.java)
	at proguard.InputReader.execute(InputReader.java)
	                        readInput(InputReader.java)
	at proguard.InputReader.execute(InputReader.java)
	                        readInput(InputReader.java)
	at proguard.ProGuard.readInput(ProGuard.java)
	at proguard.ProGuard.execute(ProGuard.java)
	                     optimize(ProGuard.java)
	                     createPrintStream(ProGuard.java)
	                     closePrintStream(ProGuard.java)
	                     fileName(ProGuard.java)
	at proguard.ProGuard.main(ProGuard.java)
Caused by: java.io.IOException: No such file or directory
	at proguard.io.DirectoryPump.pumpDataEntries(DirectoryPump.java)
	                             readFiles(DirectoryPump.java)
```	

#Detailed guides
[ProGuard](http://developer.android.com/tools/help/proguard.html#enabling-gradle)  
[ProGuard Manual](http://proguard.sourceforge.net/manual/)  
