include prorab.mk

this_name := parsexml

this_cxxflags += -Wall
this_cxxflags += -Wno-format #no warnings about format
this_cxxflags += -Wno-format-security #no warnings about format
this_cxxflags += -DDEBUG
this_cxxflags += -fstrict-aliasing #strict aliasing!!!
this_cxxflags += -std=c++11
this_cxxflags += -O3
this_cxxflags += -g

this_srcs += main.cpp

this_ldlibs += -lpapki -lmikroxml -lstdc++
this_ldflags += -L$(d)../../src

ifeq ($(os), linux)
    this_ldlibs +=
else ifeq ($(os), macosx)
    this_cxxflags += -stdlib=libc++ #this is needed to be able to use c++11 std lib
    this_ldlibs += -lc++
else ifeq ($(os),windows)
    this_cxxflags += -I/usr/include -I/usr/local/include
    this_ldflags += -L/usr/lib -L/usr/local/lib
    this_ldlibs +=
endif


$(eval $(prorab-build-app))

this_dirs := $(subst /, ,$(d))
this_test := $(word $(words $(this_dirs)),$(this_dirs))

this_xmls := $(patsubst %.xml,%,$(shell ls $(d)testdata/*.xml))

define this_rules
test:: $(prorab_this_name)
	@prorab-running-test.sh $(this_test)
	$(prorab_echo)cp $(d)../../src/*.dll $(d) || true
	$(prorab_echo)for f in $(this_xmls); do \
	    echo "checking file $$$$f.xml..." && \
	    LD_LIBRARY_PATH=$(d)../../src DYLD_LIBRARY_PATH=$$$$LD_LIBRARY_PATH $(d)./parsexml $$$$f.xml && \
	    cmp out.xml $$$$f.xml.cmp; \
	    if [ $$$$? -ne 0 ]; then prorab-error.sh "parsed XML did not match etalon sample"; exit 1; fi \
	done
	@prorab-passed.sh
endef
$(eval $(this_rules))


#add dependency on libmikroxml
$(prorab_this_name): $(abspath $(d)../../src/libmikroxml$(soext))

$(eval $(call prorab-include,$(d)../../src/makefile))
