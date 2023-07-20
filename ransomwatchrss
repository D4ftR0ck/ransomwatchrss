#!/bin/bash
current_dir=$(dirname $(realpath -s $0))
curl -sL ransomwhat.telemetry.ltd/posts | jq -r '.[] | .post_title, .group_name, .discovered' > $current_dir/feedRSSlistlast

valeur_a_verifier=$(cat $current_dir/feedRSSlistlast | tail -n 1)
date=$(tail -n 4 $current_dir/monitoring.xml | head -n 1 | cut  -c 10-35 )
# ajouter une vérification date doit etre une date... sinon bug a venir
IFS=

# vérifie que le fichier a des dates différentes pour se lancer
if [ "$valeur_a_verifier" == "$date" ]; then
exit
fi

cp $current_dir/feedRSSlistlast $current_dir/feedRSSlist

#permet de retirer les étoiles de la veille de certain ransomware qui bug avec le echo de la création de tableau.
sed -i -e 's/\*/\\*/g' -e 's/&/\&amp;/g' -e 's/<em>//g' -e 's/<\/em>//g' -e 's/<strong>//g' -e 's/<\/a>//g' -e '/^$/s//NEANT/' $current_dir/feedRSSlistlast $current_dir/feedRSSlist

# creation monitoring
if [ ! -f $current_dir/monitoring.xml ]; then
touch "$current_dir/monitoring.xml"
fi

# Open or not - monitoring
if grep -q "</rss>" $current_dir/monitoring.xml;then
sed -i '$d' $current_dir/monitoring.xml
sed -i '$d' $current_dir/monitoring.xml
fi

# Si fichier monitoring vide - creation du xml
if [ $(wc -l < $current_dir/monitoring.xml) -lt 2 ]; then
echo "<?xml version=\"1.0\" encoding=\"UTF-8\" ?>" > $current_dir/monitoring.xml
echo "<rss version=\"2.0\">" >> $current_dir/monitoring.xml
echo "<channel>" >> $current_dir/monitoring.xml
echo "<title>RANSOMWHAT</title>" >> $current_dir/monitoring.xml
echo "<link>http://ransomwhat.telemetry.ltd/posts</link>" >> $current_dir/monitoring.xml
echo "<description>Les nouveaux ransomware</description>" >> $current_dir/monitoring.xml
fi

file=$current_dir/feedRSSlist

while [ "$(wc -l < "$file")" -gt 0 ];do
IFS=$'\n'
declare -a valeurs
valeurs=($(tail -n 3 $current_dir/feedRSSlist))
   if [ "$date" != "${valeurs[2]}" ];then
   echo ${valeurs[0]} >> $current_dir/transit
   echo ${valeurs[1]} >> $current_dir/transit
   echo ${valeurs[2]} >> $current_dir/transit
   sed -i '$d' $current_dir/feedRSSlist
   sed -i '$d' $current_dir/feedRSSlist
   sed -i '$d' $current_dir/feedRSSlist

   else
   break
   fi
done

rm $current_dir/feedRSSlist

file2=$current_dir/transit

while [ "$(wc -l < "$file2")" -gt 0 ];do
IFS=$'\n'
declare -a valeurs
valeurs=($(tail -n 3 $current_dir/transit))
echo "<item>" >> $current_dir/monitoring.xml
echo "<title>${valeurs[0]}</title>" >> $current_dir/monitoring.xml
echo "<description>Groupe: ${valeurs[1]} Victime : ${valeurs[0]}</description>" >> $current_dir/monitoring.xml
echo "<link>http://ransomwhat.telemetry.ltd/posts</link>" >> $current_dir/monitoring.xml
echo "<pubDate>${valeurs[2]}</pubDate>" >> $current_dir/monitoring.xml
echo "</item>" >> $current_dir/monitoring.xml
sed -i '$d' $current_dir/transit
sed -i '$d' $current_dir/transit
sed -i '$d' $current_dir/transit
done

rm $current_dir/transit

# ferme le fichier monitoring
if ! grep -q "</rss>" $current_dir/monitoring.xml;then
echo "</channel>" >> $current_dir/monitoring.xml
echo "</rss>" >> $current_dir/monitoring.xml
fi
timeout 600 python3 -m http.server 8555 --bind 127.0.0.1 -d $current_dir/
pkill -9 -f 'python3 -m http.server'
