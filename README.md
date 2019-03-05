# pass-usernames

Infer username from filenames in a password-store archive.

```
find .password-store -type f | while read file;
do
        password=$(echo $file | sed 's/.gpg$//' | sed 's/^.password-store\///')
        if [ -z "$(echo $password | sed -n '/^\./p')" ]
        then
                content=$(pass show "$password")
                if [ -z "$(echo "$content" | sed -n '/^username:/p')" ]
                then
                        username=$(basename "$password")
                        echo "$content
username: $username" | pass insert -m -f "$password"
                fi
        fi

done
```
