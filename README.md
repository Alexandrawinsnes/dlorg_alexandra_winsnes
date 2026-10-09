 #!/usr/bin/env bash
  
 # To specify the Downloads folder that the script should monitor.
TARGET_DIR="/home/alexandrawinsnes/Downloads"

 # Creating directories
    # mkdir to create directories, -p to make sure the parent directories are created if they dont exist.

 mkdir -p "$TARGET_DIR"/{Docs,Images,Music,Other,PDF,Script,Text}

 # Creating files
    # touch to create the files, starts with "$TARGET_DIR"/ to make sure the file is created in my /home/alexandrawinsnes/Downloads.

touch "$TARGET_DIR"/{a,b,c}.txt "$TARGET_DIR"/cool{1..5}.pdf "$TARGET_DIR"/awesome_{1..4}.mp4 "$TARGET_DIR"/beautiful.jpg "$TARGET_DIR"/coolu.png "$TARGET_DIR"/notes.docx "$TARGET_DIR"/notes.sh "$TARGET_DIR"/pretty{1..3}.mp3
  

 # Moving my files in to the correct directories
    # mv to move the files, I did this step before I made the automated the process of moving files in to the correct directories. This step is therefore not necessary anymore.
        
mv "$TARGET_DIR"/{a,b,c}.text "$TARGET_DIR"/Text
mv "$TARGET_DIR"/cool{1..5}.pdf "$TARGET_DIR"/PDF
mv "$TARGET_DIR"/awesome_{1..4}.mp4 "$TARGET_DIR"/Other
mv "$TARGET_DIR"/coolu.png "$TARGET_DIR"/Images
mv "$TARGET_DIR"/notes.docx "$TARGET_DIR"/Docs
mv "$TARGET_DIR"/notes.sh "$TARGET_DIR"/Script
mv "$TARGET_DIR"/pretty{1..3}.mp3 "$TARGET_DIR"/Music
mv "$TARGET_DIR"/beautiful.jpg "$TARGET_DIR"/Images


 # Automatically sorting files in to the correct directories
    # inotifywait monitors the Downloads folder for new,moved or renamed files. The while loop reads the files and then the case statement identifies the file type and moves each file to the correct folder.    

inotifywait -m -r -e create,moved_to,moved_from,close_write --format '%w%f' "$TARGET_DIR" |

while IFS= read -r file
do
    case "$file" in

    *.pdf)
        mv "$file" "$TARGET_DIR"/PDF/ ;;
    *.jpg|*.png)
        mv "$file" "$TARGET_DIR"/Images/ ;;
    *.mp3)
        mv "$file" "$TARGET_DIR"/Music/ ;;
    *.mp4)
        mv "$file" "$TARGET_DIR"/Other/ ;;
    *.txt)
        mv "$file" "$TARGET_DIR"/Text/ ;;
    *.docx)
        mv "$file" "$TARGET_DIR"/Docs/ ;;
    *.sh)
        mv "$file" "$TARGET_DIR"/Script/ ;;

    esac

done



 # Created a systemd service to automate my dlorg script.
     # This is important to make the script run automatically instead of having to start it manually. I point ExecStart to my dlorg script because that is the program I want systemd to run and then WantedBy=default.target will make the service start automatically. 

[Unit]
Description=Organize files in Downloads
After=default.target
 
[Service]
Type=simple
ExecStart=%h/.local/bin/dlorg 

[Install]
WantedBy=default.target
    
