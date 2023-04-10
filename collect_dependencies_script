#!/usr/bin/env bash

ALL=0
if [ "--all" = "$1" ]
then
    ALL=1
    shift
fi


if [ -z $1 ]
then
    echo "Укажите целевой пакет"
    exit 0
fi


function get_dependencies_and_download {
    DEPENDS=$(apt-cache show $1 | grep Depends: | sed 's/|\|Depends:\|,\|([>=<]\+ [0-9a-zA-Z.:~+-]\+),\?//g')
    if [[ $2 == *$1* ]]
    then 
        return
    fi
    apt-get download $1
    DOWNLOADED_PKGS=$2_$1_
    #echo DOWNLOADED_PKGS=$DOWNLOADED_PKGS
    for VAR in $DEPENDS
    do
        if [ 0 -eq $ALL ]
        then
            IS_INSTALLED=$(dpkg -l $VAR | grep ii)
            if [ 0 -eq ${#IS_INSTALLED} ]
            then
                get_dependencies_and_download $VAR $DOWNLOADED_PKGS
            fi
        else
            get_dependencies_and_download $VAR $DOWNLOADED_PKGS
        fi
    done
    
}

while [ -n "$1" ]
do
    get_dependencies_and_download $1 $DOWNLOADED_PKGS
    shift
done
