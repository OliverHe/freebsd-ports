PORTNAME=	icedtea-web
PORTVERSION=	1.8.4
DISTVERSIONPREFIX=	icedtea-web-
PORTREVISION=	14
CATEGORIES=	java www
DISTFILES=	${DISTNAME}${EXTRACT_SUFX}

MAINTAINER=	java@FreeBSD.org
COMMENT=	Free Java plug-in and Java Web Start for OpenJDK

LICENSE=	GPLv2

BUILD_DEPENDS=	zip:archivers/zip \
		rustc:lang/${RUST_DEFAULT} \
		bash:shells/bash \
		gsed:textproc/gsed \
		${HAMCREST_JAR}:java/hamcrest \
		${JUNIT_JAR}:java/junit
RUN_DEPENDS=	bash:shells/bash

MAKE_JOBS_UNSAFE=yes

GNU_CONFIGURE=	yes
USE_JAVA=	yes
JAVA_VERSION=	8
JAVA_OS=	native
JAVA_VENDOR=	openjdk
USE_PERL5=	build
USE_XORG=	x11
USES=		autoreconf compiler:c++11-lang desktop-file-utils gmake perl5 \
		pkgconfig shebangfix xorg
USE_GITHUB=	yes
GH_ACCOUNT=	AdoptOpenJDK
GH_PROJECT=	IcedTea-Web
SHEBANG_FILES=	jrunscript.in shell-launcher/launchers.sh.in

CONFIGURE_ARGS=		--with-hamcrest="${HAMCREST_JAR}"
CONFIGURE_ARGS+=	--with-jdk-home="${JAVA_HOME}"
CONFIGURE_ARGS+=	--with-junit="${JUNIT_JAR}"
CONFIGURE_ARGS+=	--with-rhino="${RHINO_JAR}"
CONFIGURE_ARGS+=	--with-tagsoup="${TAGSOUP_JAR}"
CONFIGURE_ARGS+=	--with-itw-libs="DISTRIBUTION"
CONFIGURE_ARGS+=	--enable-shell-launchers
.for opt in asm chrome chromium ecj epiphany firefox jacoco midori opera
CONFIGURE_ARGS+=	--without-${opt}
.endfor
CONFIGURE_ENV=		BIN_BASH=${bash_CMD}
CONFIGURE_ENV+=		bashcompdir=${PREFIX}/etc/bash_completion.d
CONFIGURE_SHELL=	${bash_CMD}

TEST_TARGET=	check

# Upstream archive contains files with UTF-8 names
EXTRACT_CMD=	${SETENV} LC_ALL=en_US.UTF-8 /usr/bin/bsdtar

OPTIONS_DEFINE=	DOCS PLUGIN RHINO TAGSOUP
OPTIONS_DEFAULT=PLUGIN
OPTIONS_SUB=	yes

PLUGIN_DESC=	Enable the browser plug-in
RHINO_DESC=	Add support for Proxy Auto Config files
TAGSOUP_DESC=	Enable cleaning up of malformed JNLP files

DOCS_CONFIGURE_ENABLE=	docs
DOCS_PORTDOCS=		netx

RHINO_BUILD_DEPENDS=	${JAVASHAREDIR}/rhino/rhino.jar:lang/rhino
RHINO_RUN_DEPENDS=	${RHINO_BUILD_DEPENDS}
RHINO_VARS=		RHINO_JAR="${JAVASHAREDIR}/rhino/rhino.jar"
RHINO_VARS_OFF=		RHINO_JAR=no

TAGSOUP_BUILD_DEPENDS=	${JAVALIBDIR}/tagsoup.jar:textproc/tagsoup
TAGSOUP_RUN_DEPENDS=	${TAGSOUP_BUILD_DEPENDS}
TAGSOUP_VARS=		TAGSOUP_JAR="${JAVALIBDIR}/tagsoup.jar"
TAGSOUP_VARS_OFF=	TAGSOUP_JAR=no

PLUGIN_CONFIGURE_ENABLE=native-plugin pluginjar
PLUGIN_CONFIGURE_ENV=	MOZILLA_CFLAGS="-I${WRKDIR}/npapi-headers -DXP_UNIX" \
			MOZILLA_LIBS=" " # needs to be set and non-empty!
PLUGIN_DISTFILES=	libxul-npapi-headers-45.9.0${EXTRACT_SUFX}:npapi
PLUGIN_MASTER_SITES=	LOCAL/tobik:npapi
PLUGIN_PORTDOCS=	plugin
PLUGIN_TEST_TARGET=	plugin-tests
PLUGIN_USE=		GNOME=glib20
PLUGIN_USES=		webplugin:native gnome
PLUGIN_VARS=		WEBPLUGIN_DIR=${PREFIX}/lib \
			WEBPLUGIN_FILES=IcedTeaPlugin.so

HAMCREST_JAR=	${JAVALIBDIR}/hamcrest.jar
JUNIT_JAR=	${JAVALIBDIR}/junit.jar

.include <bsd.port.pre.mk>

.if ${JAVA_PORT_VERSION:M*6*}
pre-extract:
	@${ECHO_MSG}
	@${ECHO_MSG} "IMPORTANT: To build IcedTea-Web ${PORTVERSION}, you have to turn on 'ICEDTEA' option"
	@${ECHO_MSG} "for ${JAVA_PORT} (default).  Otherwise, it will neither build nor work."
	@${ECHO_MSG}
.  if !(defined(PACKAGE_BUILDING) || defined(BATCH))
	@sleep 5
.  endif
.endif

post-patch:
	${REINPLACE_CMD} -e 's|^sed\([[:space:]]\)|gsed\1|' \
	    -e 's|\([[:space:]]\)sed\([[:space:]]\)|\1gsed\2|g' \
	    ${WRKSRC}/Makefile.am ${WRKSRC}/html-gen.sh
	${REINPLACE_CMD} -e 's|^Icon=javaws|Icon=itweb-javaws|' \
	    ${WRKSRC}/*.desktop.in

post-install:
	${INSTALL_DATA} ${WRKSRC}/*.desktop ${STAGEDIR}${DESKTOPDIR}
.for lang in cs de en pl
.for man in javaws policyeditor
	${MV} ${STAGEDIR}${MANPREFIX}/man/${lang:Nen}/man1/${man}.1 \
	    ${STAGEDIR}${MANPREFIX}/man/${lang:Nen}/man1/itweb-${man}.1
.endfor
.endfor
	@${ECHO_MSG}
	@${ECHO_MSG} "Run \"make test\" to execute regression test."
	@${ECHO_MSG}

.include <bsd.port.post.mk>
