ACLOCAL_AMFLAGS = ${ACLOCAL_FLAGS}
AM_MAKEFLAGS = --no-print-directory

SUBDIRS = .

rootprefix=@rootprefix@
rootbindir=$(rootprefix)/bin
rootlibexecdir=$(rootprefix)/lib/systemd

pkgsysconfdir=$(sysconfdir)/systemd
userunitdir=$(prefix)/lib/systemd/user
userpresetdir=$(prefix)/lib/systemd/user-preset
systemunitdir=$(rootprefix)/lib/systemd/system
systempresetdir=$(rootprefix)/lib/systemd/system-preset
dbussessionservicedir=@dbussessionservicedir@

AM_V_M4 = $(AM_V_M4_$(V))
AM_V_M4_ = $(AM_V_M4_$(AM_DEFAULT_VERBOSITY))
AM_V_M4_0 = @echo "  M4      " $@;

AM_V_RM = $(AM_V_RM_$(V))
AM_V_RM_ = $(AM_V_RM_$(AM_DEFAULT_VERBOSITY))
AM_V_RM_0 = @echo "  RM      " $@;

INSTALL_DIRS =
EXTRA_DIST =
INSTALL_EXEC_HOOKS =
INSTALL_DATA_HOOKS =
UNINSTALL_DATA_HOOKS =

RUNLEVEL1_TARGET_WANTS =
RUNLEVEL2_TARGET_WANTS =
RUNLEVEL3_TARGET_WANTS =
RUNLEVEL4_TARGET_WANTS =
RUNLEVEL5_TARGET_WANTS =
SHUTDOWN_TARGET_WANTS =
LOCAL_FS_TARGET_WANTS =
MULTI_USER_TARGET_WANTS =
SYSINIT_TARGET_WANTS =
SOCKETS_TARGET_WANTS =
TIMERS_TARGET_WANTS =

SYSTEM_UNIT_ALIASES =
USER_UNIT_ALIASES =
GENERAL_ALIASES =

in_files = $(filter %.in,$(EXTRA_DIST))
in_in_files = $(filter %.in.in, $(in_files))
m4_files = $(filter %.m4,$(EXTRA_DIST) $(in_files:.m4.in=.m4))

CLEANFILES = $(BUILT_SOURCES) \
	$(pkgconfigdata_DATA) \
	$(pkgconfiglib_DATA) \
	$(in_files:.in=) $(in_in_files:.in.in=) \
	$(m4_files:.m4=)

INSTALL_DIRS += \
	$(dbussessionservicedir) \
	$(sysconfdir)/xdg/systemd \
	$(pkgsysconfdir)/system \
	$(pkgsysconfdir)/user \
	$(pkgsysconfdir)/system/multi-user.target.wants \
	$(pkgsysconfdir)/system/getty.target.wants

install-directories-hook:
	$(MKDIR_P) $(addprefix $(DESTDIR),$(INSTALL_DIRS))

substitutions = \
		'|rootlibexecdir=$(rootlibexecdir)|' \
		'|SYSTEMCTL=$(rootbindir)/systemctl|' \
		'|QUOTAON=$(QUOTAON)|' \
		'|DEBUGTTY=$(DEBUGTTY)|' \
		'|SUSHELL=$(SUSHELL)|' \
		'|KMOD=$(KMOD)|' \
		'|KILL=$(KILL)|' \
		'|pkgsysconfdir=$(pkgsysconfdir)|' \
		'|systemunitdir=$(systemunitdir)|' \
		'|userunitdir=$(userunitdir)|'

SED_PROCESS = \
	$(AM_V_GEN)$(MKDIR_P) $(dir $@) && \
	$(SED) $(subst '|,-e 's|@,$(subst =,\@|,$(subst |',|g',$(substitutions)))) \
		< $< > $@

install-aliases-hook:
	set -- $(SYSTEM_UNIT_ALIASES) && \
		dir=$(systemunitdir) && $(install-aliases)
	set -- $(USER_UNIT_ALIASES) && \
		dir=$(userunitdir) && $(install-aliases)
	set -- $(GENERAL_ALIASES) && \
		dir= && $(install-aliases)

define install-aliases
	while [ -n "$$1" ]; do \
		$(MKDIR_P) `dirname $(DESTDIR)$$dir/$$2` && \
		rm -f $(DESTDIR)$$dir/$$2 && \
		$(LN_S) $$1 $(DESTDIR)$$dir/$$2 && \
		shift 2 || exit $$?; \
	done
endef

install-target-wants-hook:
	what="$(RUNLEVEL1_TARGET_WANTS)" && wants=runlevel1.target && $(add-wants)
	what="$(RUNLEVEL2_TARGET_WANTS)" && wants=runlevel2.target && $(add-wants)
	what="$(RUNLEVEL3_TARGET_WANTS)" && wants=runlevel3.target && $(add-wants)
	what="$(RUNLEVEL4_TARGET_WANTS)" && wants=runlevel4.target && $(add-wants)
	what="$(RUNLEVEL5_TARGET_WANTS)" && wants=runlevel5.target && $(add-wants)
	what="$(SHUTDOWN_TARGET_WANTS)" && wants=shutdown.target && $(add-wants)
	what="$(LOCAL_FS_TARGET_WANTS)" && wants=local-fs.target && $(add-wants)
	what="$(MULTI_USER_TARGET_WANTS)" && wants=multi-user.target && $(add-wants)
	what="$(SYSINIT_TARGET_WANTS)" && wants=sysinit.target && $(add-wants)
	what="$(SOCKETS_TARGET_WANTS)" && wants=sockets.target && $(add-wants)
	what="$(TIMERS_TARGET_WANTS)" && wants=timers.target && $(add-wants)
	what="$(SLICES_TARGET_WANTS)" && wants=slices.target && $(add-wants)

define add-wants
	[ -z "$$what" ] || ( \
	  dir=$(DESTDIR)$(systemunitdir)/$$wants.wants && \
	  $(MKDIR_P) -m 0755 $$dir && \
	  cd $$dir && \
	  rm -f $$what && \
	  for i in $$what; do $(LN_S) ../$$i . || exit $$? ; done )
endef

INSTALL_EXEC_HOOKS += \
	install-target-wants-hook \
	install-directories-hook \
	install-aliases-hook

dist_systemunit_DATA = \
	default/graphical.target \
	default/multi-user.target \
	default/emergency.service \
	default/emergency.target \
	default/sysinit.target \
	default/basic.target \
	default/getty.target \
	default/halt.target \
	default/kexec.target \
	default/local-fs.target \
	default/local-fs-pre.target \
	default/initrd.target \
	default/initrd-fs.target \
	default/initrd-root-fs.target \
	default/remote-fs.target \
	default/remote-fs-pre.target \
	default/network.target \
	default/network-online.target \
	default/nss-lookup.target \
	default/nss-user-lookup.target \
	default/hibernate.target \
	default/hybrid-sleep.target \
	default/poweroff.target \
	default/reboot.target \
	default/rescue.target \
	default/rpcbind.target \
	default/time-sync.target \
	default/shutdown.target \
	default/final.target \
	default/umount.target \
	default/sigpwr.target \
	default/sleep.target \
	default/sockets.target \
	default/timers.target \
	default/paths.target \
	default/suspend.target \
	default/swap.target \
	default/slices.target \
	default/system.slice \
	default/x-.slice \
	default/systemd-initctl.socket \
	default/systemd-shutdownd.socket \
	default/syslog.socket \
	default/dev-hugepages.mount \
	default/dev-mqueue.mount \
	default/sys-kernel-config.mount \
	default/sys-kernel-debug.mount \
	default/sys-fs-fuse-connections.mount \
	default/tmp.mount \
	default/printer.target \
	default/sound.target \
	default/bluetooth.target \
	default/smartcard.target \
	default/systemd-ask-password-wall.path \
	default/systemd-ask-password-console.path \
	default/systemd-udevd-control.socket \
	default/systemd-udevd-kernel.socket \
	default/system-update.target \
	default/initrd-switch-root.target

nodist_systemunit_DATA = \
	default/getty@.service \
	default/serial-getty@.service \
	default/console-shell.service \
	default/console-getty.service \
	default/systemd-initctl.service \
	default/systemd-shutdownd.service \
	default/systemd-remount-fs.service \
	default/systemd-update-utmp.service \
	default/systemd-update-utmp-runlevel.service \
	default/systemd-ask-password-wall.service \
	default/systemd-ask-password-console.service \
	default/systemd-sysctl.service \
	default/emergency.service \
	default/rescue.service \
	default/user@.service \
	default/systemd-hibernate.service \
	default/systemd-hybrid-sleep.service \
	default/systemd-suspend.service \
	default/systemd-halt.service \
	default/systemd-poweroff.service \
	default/systemd-reboot.service \
	default/systemd-kexec.service \
	default/systemd-fsck@.service \
	default/systemd-fsck-root.service \
	default/systemd-udevd.service \
	default/systemd-udev-trigger.service \
	default/systemd-udev-settle.service \
	default/debug-shell.service \
	default/initrd-parse-etc.service \
	default/initrd-cleanup.service \
	default/initrd-udevadm-cleanup-db.service \
	default/initrd-switch-root.service

dist_userunit_DATA = \
	default/user/default.target \
	default/user/exit.target

nodist_userunit_DATA = \
	default/user/systemd-exit.service

EXTRA_DIST += \
	default/getty@.service.m4 \
	default/serial-getty@.service.m4 \
	default/console-shell.service.m4.in \
	default/console-getty.service.m4.in \
	default/rescue.service.in \
	default/systemd-initctl.service.in \
	default/systemd-shutdownd.service.in \
	default/systemd-remount-fs.service.in \
	default/systemd-update-utmp.service.in \
	default/systemd-update-utmp-runlevel.service.in \
	default/systemd-ask-password-wall.service.in \
	default/systemd-ask-password-console.service.in \
	default/systemd-sysctl.service.in \
	default/emergency.service.in \
	default/systemd-halt.service.in \
	default/systemd-poweroff.service.in \
	default/systemd-reboot.service.in \
	default/systemd-kexec.service.in \
	default/user/systemd-exit.service.in \
	default/systemd-fsck@.service.in \
	default/systemd-fsck-root.service.in \
	default/user@.service.in \
	default/systemd-udevd.service \
	default/systemd-udev-trigger.service \
	default/systemd-udev-settle.service \
	default/debug-shell.service.in \
	default/systemd-hibernate.service.in \
	default/systemd-hybrid-sleep.service.in \
	default/systemd-suspend.service.in \
	default/quotaon.service.in \
	default/initrd-parse-etc.service.in \
	default/initrd-cleanup.service.in \
	default/initrd-udevadm-cleanup-db.service.in \
	default/initrd-switch-root.service.in

CLEANFILES += \
	default/console-shell.service.m4 \
	default/console-getty.service.m4

nodist_systemunit_DATA += \
	default/rc-local.service \
	default/halt-local.service

EXTRA_DIST += \
	default/rc-local.service.in \
	default/halt-local.service.in

# automake is broken and can't handle files with a dash in front
# http://debbugs.gnu.org/cgi/bugreport.cgi?bug=14728#8
units-install-hook:
	mv $(DESTDIR)$(systemunitdir)/x-.slice $(DESTDIR)/$(systemunitdir)/-.slice

units-uninstall-hook:
	rm -f $(DESTDIR)/$(systemunitdir)/-.slice

INSTALL_DATA_HOOKS += units-install-hook
UNINSTALL_DATA_HOOKS += units-uninstall-hook

nodist_systemunit_DATA += \
	default/systemd-modules-load.service

nodist_systemunit_DATA += \
	default/kmod-static-nodes.service

SYSINIT_TARGET_WANTS += \
	kmod-static-nodes.service

EXTRA_DIST += \
        default/systemd-modules-load.service.in \
        default/kmod-static-nodes.service.in

dist_systemunit_DATA += \
	default/systemd-tmpfiles-clean.timer

nodist_systemunit_DATA += \
	default/systemd-tmpfiles-setup-dev.service \
	default/systemd-tmpfiles-setup.service \
	default/systemd-tmpfiles-clean.service

EXTRA_DIST += \
	default/systemd-tmpfiles-setup-dev.service.in \
	default/systemd-tmpfiles-setup.service.in \
	default/systemd-tmpfiles-clean.service.in

EXTRA_DIST += \
	default/systemd-udevd.service.in \
	default/systemd-udev-trigger.service.in \
	default/systemd-udev-settle.service.in

CLEANFILES += \
	default/systemd-udevd.service \
	default/systemd-udev-trigger.service \
	default/systemd-udev-settle.service

SOCKETS_TARGET_WANTS += \
	systemd-udevd-control.socket \
	systemd-udevd-kernel.socket

SYSINIT_TARGET_WANTS += \
	systemd-udevd.service \
	systemd-udev-trigger.service

dist_systemunit_DATA += \
	default/systemd-journald.socket

nodist_systemunit_DATA += \
	default/systemd-journald.service \
	default/systemd-journal-flush.service

SOCKETS_TARGET_WANTS += \
	systemd-journald.socket

SYSINIT_TARGET_WANTS += \
	systemd-journald.service \
	systemd-journal-flush.service

EXTRA_DIST += \
	default/systemd-journald.service.in \
	default/systemd-journal-flush.service.in

dist_systemunit_DATA += \
	default/systemd-journal-gatewayd.socket

nodist_systemunit_DATA += \
	default/systemd-journal-gatewayd.service

EXTRA_DIST += \
	default/systemd-journal-gatewayd.service.in

dist_systemunit_DATA += \
	default/proc-sys-fs-binfmt_misc.automount \
	default/proc-sys-fs-binfmt_misc.mount

nodist_systemunit_DATA += \
	default/systemd-binfmt.service

EXTRA_DIST += \
	default/systemd-binfmt.service.in

SYSINIT_TARGET_WANTS += \
	systemd-binfmt.service \
	proc-sys-fs-binfmt_misc.automount

nodist_systemunit_DATA += \
	default/systemd-vconsole-setup.service

EXTRA_DIST += \
	default/systemd-vconsole-setup.service.in

SYSINIT_TARGET_WANTS += \
	systemd-vconsole-setup.service

nodist_systemunit_DATA += \
	default/systemd-quotacheck.service

EXTRA_DIST += \
	default/systemd-quotacheck.service.in

nodist_systemunit_DATA += \
	default/quotaon.service

nodist_systemunit_DATA += \
	default/systemd-random-seed.service

EXTRA_DIST += \
	default/systemd-random-seed.service.in

SYSINIT_TARGET_WANTS += \
	systemd-random-seed.service

nodist_systemunit_DATA += \
	default/systemd-backlight@.service

EXTRA_DIST += \
	default/systemd-backlight@.service.in

dist_systemunit_DATA += \
	default/cryptsetup.target \
	default/cryptsetup-pre.target

SYSINIT_TARGET_WANTS += \
	cryptsetup.target

nodist_systemunit_DATA += \
	default/systemd-logind.service \
	default/systemd-user-sessions.service

dist_systemunit_DATA += \
	default/user.slice

EXTRA_DIST += \
	default/systemd-logind.service.in \
	default/systemd-user-sessions.service.in

MULTI_USER_TARGET_WANTS += \
	systemd-logind.service \
	systemd-user-sessions.service

SYSTEM_UNIT_ALIASES += \
	poweroff.target runlevel0.target \
	rescue.target runlevel1.target \
	multi-user.target runlevel2.target \
	multi-user.target runlevel3.target \
	multi-user.target runlevel4.target \
	graphical.target runlevel5.target \
	reboot.target runlevel6.target

SYSTEM_UNIT_ALIASES += \
	systemd-logind.service dbus-org.freedesktop.login1.service

SYSTEM_UNIT_ALIASES += \
	graphical.target default.target \
	reboot.target ctrl-alt-del.target \
	getty@.service autovt@.service

USER_UNIT_ALIASES += \
	$(systemunitdir)/shutdown.target shutdown.target \
	$(systemunitdir)/sockets.target sockets.target \
	$(systemunitdir)/timers.target timers.target \
	$(systemunitdir)/paths.target paths.target \
	$(systemunitdir)/bluetooth.target bluetooth.target \
	$(systemunitdir)/printer.target printer.target \
	$(systemunitdir)/sound.target sound.target \
	$(systemunitdir)/smartcard.target smartcard.target

GENERAL_ALIASES += \
	$(systemunitdir)/remote-fs.target $(pkgsysconfdir)/system/multi-user.target.wants/remote-fs.target \
	$(systemunitdir)/getty@.service $(pkgsysconfdir)/system/getty.target.wants/getty@tty1.service \
	$(pkgsysconfdir)/user $(sysconfdir)/xdg/systemd/user \
	../system-services/org.freedesktop.systemd1.service $(dbussessionservicedir)/org.freedesktop.systemd1.service

INSTALL_DIRS += \
	$(systemunitdir)/runlevel1.target.wants \
	$(systemunitdir)/runlevel2.target.wants \
	$(systemunitdir)/runlevel3.target.wants \
	$(systemunitdir)/runlevel4.target.wants \
	$(systemunitdir)/runlevel5.target.wants

SOCKETS_TARGET_WANTS += \
	systemd-initctl.socket \
	systemd-shutdownd.socket

SYSINIT_TARGET_WANTS += \
	systemd-update-utmp.service

LOCAL_FS_TARGET_WANTS += \
	systemd-remount-fs.service \
	systemd-fsck-root.service \
	tmp.mount

MULTI_USER_TARGET_WANTS += \
	getty.target \
	systemd-ask-password-wall.path

SYSINIT_TARGET_WANTS += \
	dev-hugepages.mount \
	dev-mqueue.mount \
	sys-kernel-config.mount \
	sys-kernel-debug.mount \
	sys-fs-fuse-connections.mount \
	systemd-sysctl.service \
	systemd-ask-password-console.path

RUNLEVEL1_TARGET_WANTS += \
	systemd-update-utmp-runlevel.service
RUNLEVEL2_TARGET_WANTS += \
	systemd-update-utmp-runlevel.service
RUNLEVEL3_TARGET_WANTS += \
	systemd-update-utmp-runlevel.service
RUNLEVEL4_TARGET_WANTS += \
	systemd-update-utmp-runlevel.service
RUNLEVEL5_TARGET_WANTS += \
	systemd-update-utmp-runlevel.service

install-exec-hook: $(INSTALL_EXEC_HOOKS)

uninstall-hook: $(UNINSTALL_DATA_HOOKS) $(UNINSTALL_EXEC_HOOKS)

install-data-hook: $(INSTALL_DATA_HOOKS)

distclean-local: $(DISTCLEAN_LOCAL_HOOKS)

clean-local: $(CLEAN_LOCAL_HOOKS)
	rm -rf $(abs_srcdir)/install-tree

DISTCHECK_CONFIGURE_FLAGS = \
	--with-rootprefix=$$dc_install_base \
	--with-dbussessionservicedir=$$dc_install_base/$(dbussessionservicedir)

default/%: default/%.in
	$(SED_PROCESS)

default/user/%: default/user/%.in
	$(SED_PROCESS)

default/%: default/%.m4
	$(AM_V_M4)$(MKDIR_P) $(dir $@)
	$(AM_V_M4)$(M4) -P $(M4_DEFINES) -DFOR_SYSTEM=1 < $< > $@

default/user/%: default/%.m4
	$(AM_V_M4)$(MKDIR_P) $(dir $@)
	$(AM_V_M4)$(M4) -P $(M4_DEFINES) -DFOR_USER=1 < $< > $@
