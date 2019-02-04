This is bazel precompiled for aarch64.

Built on:
uname -a
Linux firefly 4.4.154 #24 SMP Tue Sep 25 12:47:11 CST 2018 aarch64 aarch64 aarch64 GNU/Linux

Use the following command to install
sudo cp bazel /usr/local/bin/

to build I followed the bazel install from source instructions but my device defaults were too restrictive.

exporting 
export BAZEL_JAVAC_OPTS="-J-Xms512m -J-Xmx1024m -J-Xss1024k"

did not seem to pass these flags to javac, I needed to modify

run "${JAVAC}" -classpath "${classpath}" -sourcepath ...

and add

run "${JAVAC}" -classpath "${classpath}" -J-Xms512m -J-Xmx1024m -J-Xss1024k -sourcepath ...

in scripts/bootstrap/compile.sh


after changing the above, to prevent resource exhaustion in versions between 0.14 and 0.22 I also had to modify 
compile.sh in the main directory changing bazel_build line options to limit resources. Swapping/paging to an SD card brings things to a grinding halt in short order.

--platforms=@bazel_tools//platforms:target_platform --jobs 10 --local_resources 2048,.5,1.0



Some versions of bazel won't build other versions. There are incompatible changes between:
V0.12->V0.15<br>
V0.14->V0.16<br>


What will work:
V0.12->V0.14
V0.14->V0.15.2
