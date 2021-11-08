

clean:
	rm -rf ~/.sbt
	rm -rf ~/.cache/JNA
	rm -rf ~/.cache/coursier
	
fetch_SpinalTemplateSbt:
	git clone https://github.com/SpinalHDL/SpinalTemplateSbt.git &&	(cd SpinalTemplateSbt && sbt compile) && rm -rf SpinalTemplateSbt
	
fetch_VexRiscv_dev:
	git clone https://github.com/SpinalHDL/SpinalHDL.git --branch dev && git clone https://github.com/SpinalHDL/VexRiscv.git --branch dev && \
	(cd VexRiscv && sbt compile) && rm -rf SpinalHDL VexRiscv
	
fetch_all: fetch_SpinalTemplateSbt fetch_VexRiscv_dev

collect: 
	rm -rf home
	mkdir home
	cp -rf ~/.sbt home
	mkdir home/.cache
	cp -rf ~/.cache/JNA home/.cache
	cp -rf ~/.cache/coursier home/.cache
	
compress:		
	rm home.tgz && (cd home && tar -cvzf ../compressed.tgz .cache .sbt)
	
decompress:
	rm -rf home && mkdir home && cd home && tar xf ../home.tgz
	 
apply:
	rsync -a home/ ~
