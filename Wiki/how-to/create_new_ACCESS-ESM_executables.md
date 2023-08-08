# How to build a new ACCESS-ESM executable

## Situation
A student has come to me because they need executables for the ACCESS-ESM model with changed orbital parameters. They call the experiment '8.2ka_event'

Here's the list of orbital parameters they need:

| Parameter                          | Value             |
| ---------------------------------- | ----------------- |
| eccentricity                       | 0.019199          |
| obliquity                          | 24.222° x pi/180  |
| perihelion-180°                    | 319.495° x pi/180 |
| Date of vernal equinox             | Mar 21st noon     |
| Time of perihelion passage in days | 218.05            |

## Step 1: Check out the build environment

In a suitable directory, preferably on `/scratch` on gadi, run the commands:

```bash
# Clone the repository for the build environment
git clone https://github.com/coecms/access-esm-build-gadi.git
cd access-esm-build-gadi
# Create a new branch to ensure that changes don't go back up
git checkout -b '8.2ka_event'
```

## Step 2: Make a copy of the UM sources since we need to modify them

You can check the location of the UM repositories in the `Makefile`, but we need to now make a copy of it:

```bash
svn cp \
file:///g/data/access/access-svn/cmip5/branches/dev/jxs599/trunk_ESM1.5 \
file:///g/data/access/access-svn/cmip5/branches/dev/hxw599/8.2ka_event \
-m 'create branch for 8.2ka event experiment'
```

Here, the target branch contains my username (`hxw599`) and a proper name (`8.2ka_event`). You will have to use your own values for that.

We also need to change these values in the Makefile.
```bash
vim Makefile
```

Edit line 6 from
```
SUBMODELS_REPO=file:///g/data/access/access-svn/cmip5/branches/dev/jxs599/trunk_ESM1.5/submodels
```
to
```
SUBMODELS_REPO=file:///g/data/access/access-svn/cmip5/branches/dev/hxw599/8.2ka_event/submodels
```

Then we need to check out the sources:

```bash
make src/UM
```

(If we want to modify other submodels, we would need to check them out, but the orbital parameters only affect the UM.)

## Step 3: Make the modification to the source code
In this case the code modifications are quite minimal. There are just a few constants to change in a singe file.

```bash
cd src/UM
vim umbase_hg3/src/include/constant/astron.h
```
Then check in the changes:
```bash
svn ci -m 'change orbital parameters for 8.2ka experiment'
cd ../..
```

## Step 4: Make the executables
In the directory that was created in the Step 1, run the command
```bash
make
```

This will check out all the sources that haven't yet been checked out and compile the three executables. It will take a while.

## Step 5: Move the executables to where they can be used.
To keep some indications on which version of the UM source code the binary is based, I like to add the revision number to the file name.

```bash
cd src/UM
svn up
Updating '.'
At revision 795
cd ../../bin
mv um_hg3.exe-20230808 um_hg3.exe-r795-20230808
```

Ensure that the binaries are copied to where they are safe from `/scratch`'s  auto-removal. I'm copying them to a subdirectory on `/g/data/access`:

```bash
cp * /g/data/access/payu/access-esm/bin/coe/8.2ka/
chown -R $USER:access /g/data/access/payu/access-esm/bin/coe/8.2ka
```

And now the new executables are available for general use. 
