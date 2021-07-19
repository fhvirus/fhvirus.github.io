# A script for simplify deploy routines.
#!/bin/zsh
echo "Deploy message: $1"
echo "Deploy time: $(date +"%Y%m%d %T")"

starttime=$(date +"%s%N")

hugo > /dev/null
echo "Done building."


fullmes="$1 $(date +"%Y%m%d")"
git checkout master
git add .
git commit -m "$fullmes"
git push
echo "Done pushing master."

cd public
git checkout ghp
git add .
git commit -m "$fullmes"
git push
cd ..
echo "Done pushing pages."

endtime=$(date +"%s%N")
echo "Deploy done."
echo "Time elapsed: $(( ($endtime - $starttime) / 1000000 ))ms."
