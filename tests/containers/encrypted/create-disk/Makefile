include ../../../common.mk

CC_APP=/usr/bin/python3
CC_ENCLAVE_CONFIG=enclave-config.json

CC_IMAGE_SIZE=500M

CC_IMAGE_DATA=data.img
CC_IMAGE_DATA_SIZE=3G
CC_IMAGE_DATA_KEY_FILE=data.key

SGXLKL_ENV_APP_CONFIG=\
SGXLKL_HDS=${CC_IMAGE_DATA}:/data:0

VERBOSE_OPTS=SGXLKL_VERBOSE=1 SGXLKL_KERNEL_VERBOSE=1

CC_IMAGE=rootfs.img

ifeq ($(SGXLKL_VERBOSE),)
	SGXLKL_ENV_APP_CONFIG+=${VERBOSE_OPTS}
endif

.DELETE_ON_ERROR:
.PHONY: clean run-hw run-sw

clean:
	rm -f $(CC_IMAGE) ${CC_IMAGE_DATA}

$(CC_IMAGE):
	${SGXLKL_DISK_TOOL} create --size=${CC_IMAGE_SIZE} --alpine="python3" ${CC_IMAGE}

$(CC_IMAGE_DATA):
	truncate -s ${CC_IMAGE_DATA_SIZE} ${CC_IMAGE_DATA}

run-hw: $(CC_IMAGE) ${CC_IMAGE_DATA}
	${SGXLKL_ENV_APP_CONFIG} ${SGXLKL_STARTER} --enclave-config=${CC_ENCLAVE_CONFIG} --hw-debug $(CC_IMAGE)

run-sw: $(CC_IMAGE) ${CC_IMAGE_DATA}
	${SGXLKL_ENV_APP_CONFIG} ${SGXLKL_STARTER} --enclave-config=${CC_ENCLAVE_CONFIG} --sw-debug $(CC_IMAGE)
