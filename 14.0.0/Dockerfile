FROM eclipse-temurin:17-alpine

ARG vb_version=14.0.0
ARG st_version=14.0
ARG st_path=""

COPY vocbench3-${vb_version}-full.zip /tmp
RUN set -eux ; \
	mkdir /opt/vocbench3 ; \
	mkdir /opt/vocbench3/data ; \
	cd /opt/vocbench3 ; \
	unzip -q /tmp/vocbench3-${vb_version}-full.zip -d . ; \
	if [ -n "$st_path" ]; then \
	mkdir -p /tmp/zipdiff/public/vocbench3/ && \
	unzip -p "semanticturkey-${st_version}/lib/vocbench3-${vb_version}.jar" "public/vocbench3/vbconfig.js" > /tmp/zipdiff/public/vocbench3/vbconfig.js && \
	sed -i "s#^var st_path;#var st_path=\"${st_path}\"#" /tmp/zipdiff/public/vocbench3/vbconfig.js && \
	jar -uf "semanticturkey-${st_version}/lib/vocbench3-${vb_version}.jar" -C "/tmp/zipdiff" "public/vocbench3/vbconfig.js" && \
	rm -r /tmp/zipdiff ; \
	fi; \
	chmod -R u=rwX,go=rX semanticturkey-${st_version} ; \
	chmod +x semanticturkey-${st_version}/bin/* ; \
	echo 'semanticturkey.data.dir=../data/SemanticTurkeyData' > semanticturkey-${st_version}/application.properties ; \
	rm /tmp/vocbench3-${vb_version}-full.zip

WORKDIR /opt/vocbench3/semanticturkey-$st_version

EXPOSE 1979

ENTRYPOINT ["bin/semanticturkey.sh"]