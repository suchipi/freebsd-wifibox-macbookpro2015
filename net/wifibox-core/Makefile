PORTNAME=	wifibox-core
PORTVERSION=	0.12.0
CATEGORIES=	net

MAINTAINER=	pali.gabor@gmail.com
COMMENT=	Wifibox core functionality
WWW=		https://github.com/pgj/freebsd-wifibox

LICENSE=	BSD2CLAUSE
LICENSE_FILE=	${WRKSRC}/LICENSE

ONLY_FOR_ARCHS=	amd64

RUN_DEPENDS=	grub2-bhyve>0:sysutils/grub2-bhyve

OPTIONS_DEFINE=			BHYVE_PLUS

OPTIONS_SINGLE=			RECOVERY
OPTIONS_SINGLE_RECOVERY=	RECOVER_RESTART_VMM \
				RECOVER_SUSPEND_GUEST \
				RECOVER_SUSPEND_VMM \
				RECOVER_NONE

OPTIONS_DEFAULT=		RECOVER_RESTART_VMM
OPTIONS_EXCLUDE_FreeBSD_12=	BHYVE_PLUS

RECOVER_RESTART_VMM_DESC=	Restart the vmm(4) kernel module on resume
RECOVER_SUSPEND_GUEST_DESC=	Stop the guest on suspend, start on resume
RECOVER_SUSPEND_VMM_DESC=	Unload vmm(4) on suspend, and reload on resume
RECOVER_NONE_DESC=		No recovery for suspend/resume

BHYVE_PLUS_DESC=		Use bhyve+ (experimental)

.include <bsd.port.options.mk>

.if ${PORT_OPTIONS:MBHYVE_PLUS} || (${OSVERSION} < 1300000)
RUN_DEPENDS+=	bhyve+>0:sysutils/bhyve+
_BHYVE_PLUS=	yes
.endif

USE_GITHUB=	yes
GH_ACCOUNT=	pgj
GH_PROJECT=	freebsd-wifibox
GH_TAGNAME=	c351e55117cd5e638f9059b28994c839ad68a437

NO_BUILD=	yes
MAKE_ARGS+=	GUEST_ROOT=${LOCALBASE}/share/wifibox \
		GUEST_MAN=${LOCALBASE}/man/man5/wifibox-alpine.5.gz \
		VERSION=${PORTVERSION} \
		RECOVERY_METHOD=${PORT_OPTIONS:MRECOVER_*:S/RECOVER_//:tl}
.if defined(_BHYVE_PLUS)
MAKE_ARGS+=	BHYVE=${LOCALBASE}/sbin/bhyve \
		BHYVECTL=${LOCALBASE}/sbin/bhyvectl \
		VMM_KO=${KMODDIR}/vmm.ko
.endif

.include <bsd.port.mk>
