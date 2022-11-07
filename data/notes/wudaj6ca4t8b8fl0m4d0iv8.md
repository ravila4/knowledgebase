
## Networking

Find out which process is listening on a given port:

    [sudo] lsof -i :6600

## File Transfers

Transfer files from a remote host to a local machine with progress and compression:

    rsync -azP remote-user@remote-host:~/Remote/Path ~/Destination

## System

Find number of processors:

    nproc

Find your private IP address:

```
python -c "import socket; print(socket.gethostbyname(socket.gethostname()))"
```

Find your public IP address:

    curl ifconfig.io

## Troubleshooting

Burn an ISO image to a usb drive:

    dd bs=4M if=path/to/image.iso of=/dev/sdx status=progress oflag=sync

## File operations

Change permissions by type [directory/file] using `find`:

    sudo find [directory] -type [d/f] -exec chmod [privilege] {} \;

## Searching / Replacing

Recursive find-and-replace all instances of a string in a directory:

    find <mydir> -type f -exec sed -i 's/<string1>/<string2>/g' {} +

Inline search and replace:

    echo "${WORD/SEARCH/REPLACE}"

Grep PDF files:

    find /path -iname '*.pdf' -exec pdfgrep pattern {} +

## Images

Convert format for multiple images:

```bash
for file in *.jpg;
  do convert $file ${file/.jpg/.png};
done
```
Resize multiple images:

```bash
for file in *.jpg;
  do convert -quality  90 -resize 1000x1000 $file $file;
done
```

Crop multiple images ("gravity" = north, south, east, west, center):

```bash
for file in *jpg;
  do convert -extent 1000x1000 -gravity center $file $file;
done
```

OCR an image:

    tesseract my-image output.txt

## Video

Create a video from images in a folder. (Image file names must be indexed in order.)

```
ffmpeg -framerate 30 -pattern_type glob -i '*.jpg' -c:v libx264 \
  -r 30 -pix_fmt yuv420p out.mp4
```

## More PDF Stuff

Convert markdown to pdf:

    pandoc -f my-notes.md -o my-notes.pdf

OCR:

    ocrmypdf original-document.pdf new-document.pdf

