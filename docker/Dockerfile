# SpinalHDL "Batteries-Included" Docker Build Script
# by Wesley J. Landaker <wjl@icecavern.net>
# Initially created on 2021-11-04.
#
# This file is released into the public domain.
# Please feel free to use or modify in any way you wish.

# Build with something like:
# 	docker build -t spinalhdl .
#
# Test to make sure it can run without network using something like:
# 	For SpinalTemplateSbt:
# 		git clone https://github.com/SpinalHDL/SpinalTemplateSbt.git
# 		cd SpinalTemplateSbt
# 		docker run -it --rm --network=none --volume=$(pwd):/data spinalhdl sbt "runMain mylib.MyTopLevelVhdl"
# 		docker run -it --rm --network=none --volume=$(pwd):/data spinalhdl sbt "runMain mylib.MyTopLevelVerilog"
# 		docker run -it --rm --network=none --volume=$(pwd):/data spinalhdl sbt "runMain mylib.MyTopLevelSim"
#
# 	For VexRiscv:
# 		git clone https://github.com/SpinalHDL/VexRiscv.git
# 		cd VexRiscv
# 		docker run -it --rm --network=none --volume=$(pwd):/data spinalhdl sbt "runMain vexriscv.demo.GenFull"

# Start with an appropriate Ubuntu LTS as a reasonable base system.
FROM docker.io/ubuntu:20.04

# Install prerequisite packages.
# * build-essential     : building simulation models
# * curl                : Fetching repository GPG keys
# * default-jdk-headless: JDK for Scala, SBT, SpinalHDL
# * ghdl                : GHDL VHDL simulation
# * gnupg               : GPG supprt for APT
# * iverliog            : Icarus Verilog simulation
# * verilator           : Verilator simulation
RUN \
	apt-get update && \
	DEBIAN_FRONTEND=noninteractive apt-get install \
		--assume-yes \
		build-essential \
		curl \
		default-jdk-headless \
		ghdl \
		gnupg \
		iverilog \
		verilator \
	&& \
	rm -rf /var/lib/apt/lists/* && \
	true

# Install SBT from official SBT repositories.
RUN \
	printf '%s\n' 'deb https://repo.scala-sbt.org/scalasbt/debian all main' > /etc/apt/sources.list.d/sbt.list && \
	printf '%s\n' 'deb https://repo.scala-sbt.org/scalasbt/debian /'        > /etc/apt/sources.list.d/sbt_old.list && \
	curl -sL 'https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x2EE0EA64E40A89B84B2DF73499E82A75642AC823' | apt-key add && \
	apt-get update && \
	apt-get install \
		--assume-yes \
		sbt \
	&& \
	rm -rf /var/lib/apt/lists/* && \
	true

# Run SpinalHDL on some example repositories to force SBT to download SpinalHDL & dependencies.
# * SpinalTemplateSbt: official SpinalHDL template
# * VexRiscv         : customizable RISC-V processor
RUN \
	cd /tmp && \
	git clone https://github.com/SpinalHDL/SpinalTemplateSbt.git && \
	cd SpinalTemplateSbt && \
	sbt "runMain mylib.MyTopLevelVhdl" && \
	sbt "runMain mylib.MyTopLevelVerilog" && \
	sbt "runMain mylib.MyTopLevelSim" && \
	cd .. && \
	rm -rf SpinalTemplateSbt && \
	cd /tmp && \
	git clone https://github.com/SpinalHDL/VexRiscv.git && \
	cd VexRiscv && \
	sbt "runMain vexriscv.demo.GenFull" && \
	cd .. && \
	rm -rf VexRiscv && \
	true

VOLUME /data
WORKDIR /data
