 #!/usr/bin/env bash
  
    # Creating directories
 mkdir -p Downloads/Docs Downloads/Images Downloads/Music Downloads/Other Downloads/PDF Downloads/Text Downloads/Script

    # Creating files
touch Downloads/{a,b,c}.txt Downloads/cool{1..5}.pdf Downloads/awesome_{1..4}.mp4 Downloads/beautiful.jpg Downloads/coolu.png Downloads/notes.docx Downloads/notes.sh Downloads/pretty{1..3}.mp3  

    # Moving my files in to the correct directories
mv Downloads/{a,b,c}.text Downloads/Text
mv Downloads/cool{1..5}.pdf Dowloads/PDF
mv Downloads/awesome_{1..4}.mp4 Downloads/Other
mv Downloads/coolu.png Downloads/Images
mv Downloads/notes.docx Downloads/Docs
mv Downloads/notes.sh Downloads/Script
mv Downloads/pretty{1..3}.mp3 Downloads/Music
mv Downloads/beautifuk.jpg Downloads/Images


    # Automatically sorting files in to the correct directories
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
