# git-secrets-patterns
Patterns for use with github.com/awslabs/git-secrets

To add hooks to all local repositories
```
git secrets --install ~/.git-templates/git-secrets
git config --global init.templateDir ~/.git-templates/git-secrets
git secrets --register-aws --global
```
```
cat <<EOF> ~/.git-secrets-patterns
(ey)([a-zA-Z0-9_=]+)\.([a-zA-Z0-9_=]+)\.([a-zA-Z0-9_\-\+\/=]*)
(SECRET)(\s*[=\:\-]\s*)(\w.*)
(secret)(\s*[=\:\-]\s*)(\w.*)
\w*[\-_](SECRET)\s*[=\:\-]\s*.+
\w*[\-_](secret)\s*[=\:\-]\s*.+
\w*[\-_](Secret)\s*[=\:\-]\s*.+
EOF
```

git secrets --add-provider --global -- cat ~/.git-secrets-patterns

```
cat <<EOF> examples.txt
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

foo:eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

export SECRET=asdfasdfasdf
export SECRET=12123
export BOOK_SECRET=aasdfasdfasdf

export IS_A_SECRET=12123
export IS_A_SECRET=12123-23423-234234 #some secret
export NOT_A_SECRET=
export some-secret=abd
export secret=23423
secret : 

## we don't really want to put a secret here
example
EOF

```


```
git secrets --scan examples.file
```
