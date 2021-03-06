# android-adb-backup
Backing up and restoring android apps via adb root (when you can't run
TiBackup due to lack of SuperSU support)

This is not 'adb backup' which has 2 problems:
1) it skips backing up applications if they request not to be backed up.
That's a problem when it's your data and the application developer decides that 
you are now allowed to back it up or migrate it to a new device
2) adb backup has various bugs or issues depending on which version you use. 
3) Sadly newer versions of adb backup seems to refuse to backup apps that
are not in google play anymore.

These scripts kind of does what TiBackup (from google play) does, but
they're not nearly as smart.
However, they will work on any device where you have adb root, whereas
TiBackup requires SuperSU which is much harder to get working on some
devices.
As an example, google userdebug builds give you adb root, but will not
give you any way to run android apps as root, which TiBackup requires.

Intended usage:
1) run backup_apps.sh on the source device, consider moving out any
   app called com.android/com.google as they usually have good enough
   built in backups and copying their data on another device may create
   duplicate IDs and problems later
2) Optionally run backup_apps.sh and backup_system_apps.sh on the source
   device This is even more likely to cause problems unless you restore
   on the exact same kind of device and you delete never reuse the
   source device.
3) then run restore_apps.sh against the destination device. You can
   optionally give one or more package names if you don't want to
   restore all packages
4) You can optionally run restore_system_apps.sh but again, be warned
   that this is only reasonably safe if the source and destination
   devices are the same, and you delete the source device after you're
   done
   Even then, you may want to skip this step unless it is useful to your use case.

Keep in mind that even with step #3, restoring all apps may cause issues
as some have unique IDs that end up causing problems if you use the same
unique ID on different devices.
You may want to consider only restoring apps that are missing from google play or apps
that sadly decide to prevent backups of their (i.e. your) data.

Credit goes to Raphael Moll for the original idea/scripts this was based on.
