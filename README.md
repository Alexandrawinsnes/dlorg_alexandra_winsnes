 #!/usr/bin/env bash
  
 # Creating directories
    # mkdir to create directories, -p to make sure the parent directories are created if they dont exist.

 mkdir -p Downloads/Docs Downloads/Images Downloads/Music Downloads/Other Downloads/PDF Downloads/Text Downloads/Script

 # Creating files
    # touch to create the files, starts with Downloads/ to make sure the fike is created in Downloads.

touch Downloads/{a,b,c}.txt Downloads/cool{1..5}.pdf Downloads/awesome_{1..4}.mp4 Downloads/beautiful.jpg Downloads/coolu.png Downloads/notes.docx Downloads/notes.sh Downloads/pretty{1..3}.mp3  

 # Moving my files in to the correct directories
    # mv to move the files, I did this step before I made the automated the process of moving files in to the correct directories. This step is therefore not necessary anymore.
        
mv Downloads/{a,b,c}.text Downloads/Text
mv Downloads/cool{1..5}.pdf Dowloads/PDF
mv Downloads/awesome_{1..4}.mp4 Downloads/Other
mv Downloads/coolu.png Downloads/Images
mv Downloads/notes.docx Downloads/Docs
mv Downloads/notes.sh Downloads/Script
mv Downloads/pretty{1..3}.mp3 Downloads/Music
mv Downloads/beautifuk.jpg Downloads/Images


 # Automatically sorting files in to the correct directories
    # inotifywait to automatically identify when a new file is created.
    #  -m (monitor) to make sure the command run continously.
    #  -e to decide what the intoifywait should monitor. 
    #  create and moved_to means that it should react when a new file is created/a file is moved to Downloads.
    # --format '%w%f' Downloads | to decide which information we need and gives us the file path and filename. And then which folder to monitor.    

inotifywait -m Downloads -e create -e moved_to --format '%w%f' Downloads |

while read file
do
    case "$file" in

    *.pdf)
        mv "$file" Downloads/PDF/ ;;
    *.jpg|*.png)
        mv "$file" Downloads/Images/ ;;
    *.mp3)
        mv "$file" Downloads/Music/ ;;
    *.mp4)
        mv "$file" Downloads/Other/ ;;
    *.txt)
        mv "$file" Downloads/Text/ ;;
    *.docx)
        mv "$file" Downloads/Docs/ ;;
    *.sh)
        mv "$file" Downloads/Script/ ;;

    esac

done    
