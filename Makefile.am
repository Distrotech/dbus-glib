GLIB_PC=dbus-glib-1.pc

SUBDIRS=dbus test tools
DIST_SUBDIRS=dbus test tools

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = $(GLIB_PC)

DISTCLEANFILES = 		\
	$(GLIB_PC)

EXTRA_DIST =			\
	HACKING			\
	dbus-glib-1.pc.in

#all-local: Doxyfile

if DBUS_GCOV_ENABLED
clean-gcov:
	find -name "*.da" -o -name "*.gcov" | xargs rm || true

clean-bbg:
	find -name "*.bbg" -o -name "*.bb" | xargs rm || true

GCOV_DIRS=$(GLIB_SUBDIR)

## .PHONY so it always rebuilds it
.PHONY: coverage-report.txt
coverage-report.txt:
	BBG_FILES=`find $(GCOV_DIRS) -name "*.bbg" -o -name "*.gcno"` ;			\
	C_FILES= ;									\
	for F in $$BBG_FILES ; do							\
		F_nolibs=`echo $$F | sed -e 's/.libs\///g'` ;				\
		C=`echo $$F_nolibs | sed -e 's/.bbg/.c/g' | sed -e 's/.gcno/.c/g'`  ;	\
		B=`basename $$F .bbg` ;							\
		D=`dirname $$F` ;							\
		DA=`echo $$F | sed -e 's/.bbg/.da/g'` ;					\
		DA_libs=`echo $$D/.libs/$$B/.da` ;					\
		if test -e $$DA || test -e $$DA_libs; then				\
			C_FILES="$$C_FILES $$C" ;					\
		fi ;									\
	done ;										\
	echo $$C_FILES ;								\
	$(top_builddir)/test/decode-gcov --report $$C_FILES > coverage-report.txt

check-coverage: clean-gcov all check coverage-report.txt
	cat coverage-report.txt

else
coverage-report.txt:
	echo "Need to reconfigure with --enable-gcov"

check-coverage:
	echo "Need to reconfigure with --enable-gcov"

endif