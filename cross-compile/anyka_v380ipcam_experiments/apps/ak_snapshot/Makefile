BIN = $(TARGET_BIN)
CUR_PATH = $(shell echo $(PWD))
LIB_SEARCH_PATHS := \
	$(PLAT_WORKDIR)/libplat/lib
	
INCLUDES := \
    $(CUR_PATH) \
	-I$(PLAT_WORKDIR)/libplat/include \
	-I${LIBQUIRC_PATH} \
	-I${LIBALSA_PATH}

LIBS := akuio \
        akispsdk \
        plat_common \
        plat_thread \
        plat_vi \
        plat_vpss \
        plat_ipcsrv \
        pthread 

STATIC_LIBS :=
                
CFLAGS += -fno-strict-aliasing -Os -w -D_GNU_SOURCE -std=c99 -fms-extensions

SRC_DIR := src
OBJ_DIR := obj
BIN_DIR := .
SRCS := $(wildcard $(SRC_DIR)/*.c)
OBJS := $(SRCS:$(SRC_DIR)/%.c=$(OBJ_DIR)/%.o)


LDFLAGS := $(LDFLAGS) \
    $(patsubst %,-L%,$(LIB_SEARCH_PATHS)) -L/media/ricardo/Dados/Outros/Linux/IPCAM/Camera-ANYKA-v380/TEST_SRCS/quirc/lib \
    -Wl,-rpath-link=$(subst $(space),:,$(strip $(LIB_SEARCH_PATHS))) \
    -Wl,-Bstatic \
    -Wl,--start-group $(foreach n, $(STATIC_LIBS), -l$(patsubst lib%,%,$(patsubst %.a,%,$(notdir $(n))))) -Wl,--end-group \
    -Wl,-Bdynamic \
    $(foreach y, $(LIBS), -l$(patsubst lib%,%,$(patsubst %.so,%,$(notdir $(y)))))


$(OBJ_DIR)/%.o: $(SRC_DIR)/%.c | $(OBJ_DIR)
	$(CC) $(CFLAGS) $(INCLUDES) -c -o $@ $<

$(BIN): $(OBJS)
	$(CC) -o $@ $(OBJS) $(LDFLAGS)

all:$(BIN)

$(OBJ_DIR):
	mkdir -p $@

clean:
	@$(RM) -rv $(BIN) $(OBJ_DIR) # The @ disables the echoing of the command
