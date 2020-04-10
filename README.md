# A script to mount sparsebundle images

## Install

First, make `ImageMount` operate on your user's home directory, then copy it to `/usr/local`

```
sed -i -e "s;<<user>>;$HOME;" ImageMount
cp ImageMount /usr/local/bin
```

Copy the launch daemon to `/Library/LaunchDaemons`.
This has to be done as `root` because it will mount images non-removable by default.

```
sudo cp de.gerritbeine.ImageMount.plist /Library/LaunchDaemons
```

Create the image dir and set up a test mount.

```
mkdir ~/.images
mkdir ~/testmnt
hdiutil create -size 5g -fs HFS+J -volname "test" ~/.images/test.sparsebundle
echo "test.sparsebundle,${HOME}/testmnt" >> ~/.images/images.conf
sudo /usr/local/bin/ImageMount start removeable
```

If everything is set up correctly, you will see something like

```
/dev/disk4          	GUID_partition_scheme          	
/dev/disk4s1        	EFI                            	
/dev/disk4s2        	Apple_HFS                      	/youhomedir/testmnt
```

Now, umount the test and create real images.

```
diskutil umount ~/testmnt
```
