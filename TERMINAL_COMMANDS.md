() - means the command with args from a tree. Used to minimize repetitions.  
E.g.: cmd -> arg -> subarg -> () --flag - Here () means 'cmd arg subarg'  
and the whole command is: 'cmd arg subarg --flag'

## Contents <placehodler>
ffmpeg
less
more
qpdf

## ffmpeg

```text
# Basic compression of mp4 file to make it smaller (crf 18 Very-HQ, 23 default, 28 smaller file)
ffmpeg -i input.mp4 -vcodec libx264 -crf 28 output.mp4

# Smaller file. More aggressive compression (-preset slow - better compression, slower encoding)
ffmpeg -i input.mp4 -vcodec libx264 -crf 32 -preset slow output.mp4

# Change resolution - scales to 1280px width and keeps the aspect ratio
ffmpeg -i input.mp4 -vf scale=1280:-1 -crf 28 output.mp4

# Limit bitrate
ffmpeg -i input.mp4 -b:v 1000k -b:a 128k output.mp4

# Cut first N (10) seconds (-c copy. No re-encoding, no quality loss. May not be precise):
ffmpeg -ss 10 -i input.mp4 -c copy output.mp4

# Precise cut (reencoding)
ffmpeg -i input.mp4 -ss 10 -c:v libx264 -crf 23 -c:a aac output.mp4

# Cut + compression
ffmpeg -ss 10 -i input.mp4 -vcoded libx264 -crf 28 output.mp4

# Cut a segment
ffmpeg -ss 10 -t 30 -i in.mp4 -c copy out.mp4
```

## less

```text
() -R	- Print text with ASCII control characters interpreted (e.g. color).
```

## more
Older version of 'less'. No option for going back. Use 'less' instead.

## qpdf

Requires installation.

```text
# Page count
qpdf --show-npages file.pdf

# Full metadata + structure check
qpdf --check file.pdf

# Show file info
qpdf --show-encryption file.pdf

# Extract pages
qpdf input.pdf --pages input.pdf 3-5  # range
qpdf input.pdf --pages input.pdf 3 5 10  # specific
qpdf input.pdf --pages input.pdf 5 1 9 3  # reorder

# Merge/split
qpdf --empty --pages a.pdf b.pdf c.pdf -- output.pdf
qpdf --linearize input.pdf output.pdf  # requires additional anlaysis

# Encryption/decryption
qpdf --password=PASS --decrypt input.pdf output.df
qpdf --encrypt userpass ownerpass 256 --input.pdf output.pdf  # user, owner, key-length
qpdf --stream-data=compress input.pdf output.pdf
qpdf --object-streams=generate --compress-streams=y input.pdf output.pdf

# Page manipulation
qpdf input.pdf --rotate=+90:2-5 -- output.pdf
```
