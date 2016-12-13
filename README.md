# adb_python
import datetime
import os

os.system("adb root")
os.system("adb remount")
#local_path = 'N:\\GraceQualcommMMREL\\android\\out\\target\\product\\graceqltevzw\\system'
#local_path = 'Z:\\HeroQ\\android\\out\\target\\product\\heroqltevzw\\system'
#local_path = 'Z:\\A5\\android\\out\\target\\product\\a5y17lte\\system'
local_path = 'V:\\Workspace\\4_DREAM\\DREAM2QLTE-USA-SINGLE_1167142_NM\\android\\out\\target\\product\\dream2qltesq\\system'
now_time = datetime.datetime.today()
dt = datetime.timedelta(minutes = 20)
up_dt = now_time + dt
down_dt = now_time - dt
counter = 0
for (path, dir, files) in os.walk(local_path):
    for filename in files:
        ext = os.path.splitext(filename)[-1]
        if ext == '.so':
            mtime = os.path.getmtime(path+'\\'+filename)
            filetime = datetime.datetime.fromtimestamp(mtime)
            if down_dt < filetime < up_dt:
                counter += 1
                nIndex = path.find('system')
                android_path = path[nIndex:]
                remove_android_path = android_path.split("\\")
                change_android_path = '/'.join(remove_android_path)
                commend_push = 'adb push '+path+'\\'+filename +' /'+change_android_path
                print(commend_push)
                os.system(commend_push)
                print('file is pushed')
if counter != 0:
	print('push end reboot')
	os.system('adb reboot')
	os.system('pause')
else:
	print('nothing changed time between = '+dt)
