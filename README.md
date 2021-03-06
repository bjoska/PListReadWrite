# PListRW

A Rubymotion module to make light work of writing to and reading from plist files.

## Installation

When I'm creating Ruby modules to share across more than one app I prefer to install the modules as git submodules.
That way if I need to update/patch I only need to update once, not in every app!

    # From your project root dir
    git submodule add git@github.com:Bodacious/PListReadWrite.git lib/plist_rw
    
Alternatively you can just copy/paste the file directly to your app.

## Example Usage

Lets assume we have this plist file in our resources directory...

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>jim</key>
    <dict>
      <key>name</key>
      <string>Jim</string>
      <key>email</key>
      <string>jim@jimsemail.com</string>
    </dict>
    <key>jane</key>
    <dict>
      <key>name</key>
      <string>Jane</string>
      <key>email</key>
      <string>jane@janesemail.com</string>
    </dict>
  </dict>
</plist>
```

... and we want to copy it over to our app's documents directory so we can update/edit the plist.

``` ruby

# Check if the plist exists in our documents dir
PListRW.exist?(:users, directory = :documentsDir) # => false

# Check if the plist exists in our main bundle
PListRW.exist?(:users, directory = :mainBundle) # => true

# Copy the plist file from the main bundle to the documents dir
PListRW.copyPlistFileFromBundle(:users)

# Fetch the object from the plist file
@users_hash = PListRW.plistObject(:users, Hash) # => A hash containing the User data

# Update the data
@users_hash[:jim] # => { name: "Jim", email: "jim@jimsemail.com" }
@users_hash[:jim][:name] = 'James'

# Store the data back in the plist
PListRW.updatePlistFileWithObject(:users, @users_hash)

# Check this worked OK
PListRW.plistObject(:users, Hash)[:jim][:name] # => "James"
```