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


The scan runs on commit, not add:

```
user@machine  ~/sources/some_repo   main ✚  git commit -m 'Add secrets file as test'
test-secrets.txt:1:eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
test-secrets.txt:3:foo:eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
test-secrets.txt:5:export SECRET=asdfasdfasdf
test-secrets.txt:6:export SECRET=12123
test-secrets.txt:7:export BOOK_SECRET=aasdfasdfasdf
test-secrets.txt:9:export IS_A_SECRET=12123
test-secrets.txt:10:export IS_A_SECRET=12123-23423-234234 #some secret
test-secrets.txt:12:export some-secret=a

[ERROR] Matched one or more prohibited patterns

Possible mitigations:
- Mark false positives as allowed using: git config --add secrets.allowed ...
- Mark false positives as allowed by adding regular expressions to .gitallowed at repository's root directory
- List your configured patterns: git config --get-all secrets.patterns
- List your configured allowed patterns: git config --get-all secrets.allowed
- List your configured allowed patterns in .gitallowed at repository's root directory
- Use --no-verify if this is a one-time false positive
```
