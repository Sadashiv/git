Deleting the objects
====================

Cleaning up the old objects
---------------------------
cd gitepo
ls .git/objects/pack/ 
pack-38ca2c173f154aa93a018d4bab87a4a9b484096a.idx pack-38ca2c173f154aa93a018d4bab87a4a9b484096a.pack

git verify-pack -v .git/objects/pack/pack-7b03cc896f31b2441f3a791ef760bd28495697e6.idx | sort -k 3 -n | tail -10

blob 6979 670 156343 7647cbbe10e7638d71079fd8f6ca295e89941a51
blob 10107 3726 68483 050c88e2b648e1f9622f56cdff28005b8724fadd
blob 12302 4462 186227 3b550a4b0111d8befa6272cd5d11ed8a7f545779
blob 13596 4808 171955 3280709c2f835e562b99a75180e858f450a429aa
blob 15167 6558 118383 ee8eb338da07df093cd2cfb4cb09337b6031bc36
blob 15335 6243 128430 24fa51b794dc3dce8982baf21343f09c66df1261
blob 16977 5795 160572 79e2d4ec85799442dd158343d351fd3c3be112af
blob 19201 5188 93224 e1b1eaf0e38faf452366216d61ff49895a5579ab
blob 35858 6332 134997 2e1b5e14b92fe61e02df94ab165c8aab6c06c9cb 

git rev-list --objects --all | grep 2e1b5e14b92fe61e02df94ab165c8aab6c06c9cb
git filter-branch --index-filter 
git rm --cached --ignore-unmatch *.pdf' --all
rm -Rf .git/refs/original
rm -Rf .git/logs/
git gc --aggressive --prune=now

