Files in the repo root:
- mp3-archive.tar.part-aa … part-ay — 25 volumes, 95 MB each (last one is smaller), 2.3 GB total
- mp3-archive.tar.SHA256SUMS — checksums for all 25 parts

Verification passed: reassembling all parts lists 366 entries (365 mp3s + the directory), and a sample file extracted from the archive is byte-identical (matching SHA-256) to the original.

To commit and push:
git add mp3-archive.tar.part-* mp3-archive.tar.SHA256SUMS
git commit -m "Add split tar archive of mp3 directory"
git push

To restore after download:
shasum -a 256 -c mp3-archive.tar.SHA256SUMS   # optional integrity check
cat mp3-archive.tar.part-* | tar xf -

One thing worth flagging before you push: the 365 mp3s are already tracked and committed in this repo (.git is 2.2 GB, remote git@github.com:wenbo/english_pod_mp3_only.git). Committing the archive adds a second 2.3 GB copy of the same audio, pushing the repo toward ~4.6 GB — GitHub warns above 1 GB and its soft limit is 5 GB, so the push may be slow or rejected. Options if you'd rather not double up:

- git rm -r --cached mp3/ and add mp3/ to .gitignore, so only the archive is tracked (history still holds the old blobs unless rewritten with git filter-repo).
- Attach the parts to a GitHub Release instead of committing them (gh release create v1 mp3-archive.tar.part-*) — release assets don't count against repo size.

The archive files are on disk either way; say the word if you want me to set up either option.


## reply
gh release create mp3-v1 mp3-archive.tar.part-* mp3-archive.tar.SHA256SUMS \
  --title "EnglishPod MP3 archive" \
  --notes "Split tar archive. Restore with: cat mp3-archive.tar.part-* | tar xf -"
