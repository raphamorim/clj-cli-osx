# clj-cli-osx

Installing Clojure on Mac OS X 

## Dependencies

Considering you all ready have Java and Brew installed.

## Scripting

Add your PATH in `~/.profile` and update: 

```sh
export PATH="/usr/local/bin:/usr/local/sbin:$PATH"
```

```sh
source ~/.profile
```

Donwload and unzip Clojure

```sh
curl -O -J -L http://repo1.maven.org/maven2/org/clojure/clojure/1.7.0/clojure-1.7.0.zip
```

After unzip open clojure folder and test the build

```sh
java -cp clojure-1.1.0.jar clojure.main
Clojure 1.1.0
user=> (+ 1 1)
2
user=> (System/exit 0)
```

Let's move jar file

```sh
mkdir /usr/local/lib/clojure && cp clojure-1.7.0.jar /usr/local/lib/clojure/clojure.jar
```

rlwrap will make interacting with the Clojure REPL much more friendly. 
It is not necessary but I recommend it. With rlwrap you will have tab completion, parenthesis matching, and much more.

```sh
brew install rlwrap
```

Create clj script

```sh
vim /usr/local/bin/clj
```

Copy the following text into the file:

```sh
#!/bin/sh
# Parts of this file come from:
# http://en.wikibooks.org/wiki/Clojure_Programming/Getting_Started#Create_clj_Script 

BREAK_CHARS="\(\){}[],^%$#@\"\";:''|\\"
CLOJURE_DIR=/usr/local/lib/clojure
CLOJURE_JAR=$CLOJURE_DIR/clojure.jar
CLASSPATH="$CLOJURE_DIR/*:$CLOJURE_JAR"

while [ $# -gt 0 ]
do
    case "$1" in
    -cp|-classpath)
            CLASSPATH="$CLASSPATH:$2"
    shift ; shift
    ;;
-e) tmpfile="/tmp/`basename $0`.$$.tmp"
    echo "$2" > "$tmpfile"
    shift ; shift
    set "$tmpfile" "$@"
    break # forces any -cp to be before any -e
    ;;
*)  break
    ;;
esac
done

if [ $# -eq 0 ]
then
  exec rlwrap --remember -c -b $BREAK_CHARS \
          java -cp $CLASSPATH clojure.main
else
  exec java -cp $CLASSPATH clojure.main $1 -- "$@"
fi
```

Make the script executable:

```sh
sudo chmod +x /usr/local/bin/clj
```

Now you should be able to run Clojure from the command line.

```sh
clj
```

clj REPL:

```sh
user=> (+ 3 5)
8
user=> (def A [[0 1 2] [3 4 5] [6 7 8]])
#'user/A
user=> (let [[x y z] (first A)]
            (println (+ x y z)))
3
nil
user=> (println "Hello, World!")
Hello, World!
nil
```

clj file:

```sh
clj clojure_script.clj
```

---------------------------

Credits: [Waratuman](http://www.waratuman.com/2010/02/18/setting-up-clojure/)
