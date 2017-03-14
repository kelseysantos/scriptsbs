#!/bin/bash
#################################################################################
# WWW.KELSEYSANTOS.COM.BR - contato:kelseysantos@yahoo.com.br
###########################################################################INICIO
# nano /etc/init.d/INSTALLVNCUBUNTU1604 && chmod +x /etc/init.d/INSTALLVNCUBUNTU1604 && echo "alias INSTALLVNCUBUNTU1604='/etc/init.d/INSTALLVNCUBUNTU1604'" >> /root/.bashrc
MESQRODOU=$(date +%h.%Y)
DATAQRODOU=$(date +%d%m%y-%H)
NOMELOG="INSTALLVNCUBUNTU1604"
NOMEPASTADELOG=LOGS #pasta de visualizacao web
PASTAWWW=/var/www/html #depende da versao
PASTASCRIPTS=/var/log/$NOMELOG

CONFGERAL(){
#Instrucao -ctime = criado e -mtime = modificado e -atime = acessado
[ -d $PASTASCRIPTS ] && find $PASTASCRIPTS -mtime +30 -exec rm -frv {} \;
[ ! -d $PASTASCRIPTS ] && mkdir -p $PASTASCRIPTS
[ ! -d $PASTAWWW/$NOMEPASTADELOG ] && mkdir -p $PASTAWWW/$NOMEPASTADELOG
[ ! -d $PASTAWWW/$NOMEPASTADELOG/$NOMELOG ] && ln -sf $PASTASCRIPTS/ $PASTAWWW/$NOMEPASTADELOG/$NOMELOG
[ ! -d $PASTASCRIPTS/$MESQRODOU/$DATAQRODOU ] && mkdir -p $PASTASCRIPTS/$MESQRODOU/$DATAQRODOU
LOGS=$PASTASCRIPTS/$MESQRODOU/$DATAQRODOU/$NOMELOG-$( date +%d%m%y ).txt
chmod -R 755 $PASTASCRIPTS
}
##############################################################################FIM

INSTALLVNC(){
apt-get install -y dialog
clear
VNCSERVICE=/etc/systemd/system/x11vnc.service
#apt-get purge --remove -y x11vnc x11vnc-data vnc-java libvncserver0
apt-get install -y x11vnc

#Definindo variavel
VNCSENHA=$(tempfile 2>/dev/null)
trap "rm -f $VNCSENHA" 0 1 2 5 15

#aqui que pega a senha
dialog --backtitle "Ten Kelsey - Comando Militar do Leste" \
--title "Alterando Senha do Acesso Remoto" \
--clear \
--insecure \
--passwordbox "Por favor Digite a senha do Acesso Remoto" 10 50 2> $VNCSENHA

ret=$?

#vamos decidir o que fazer
case $ret in
  0)
    x11vnc -storepasswd $(cat $VNCSENHA) /etc/x11vnc.pass
    ;;
  1)
    echo "A instalacao foi cancelada."
    ;;
  255)
    [ -s $VNCSENHA ] &&  cat $VNCSENHA || echo "A instalacao foi cancelada."
    ;;
esac

#UBUNTU 16.04 CRIANDO O SERVICO VNC
[ -d /etc/x11vnc.pass ] && chmod 744 /etc/x11vnc.pass
[ -d $VNCSERVICE ] && systemctl stop x11vnc;rm -f $VNCSERVICE;touch $VNCSERVICE
echo "[Unit]" >> $VNCSERVICE
echo "Description=\"x11vnc\"" >> $VNCSERVICE
echo "Requires=display-manager.service" >> $VNCSERVICE
echo "After=display-manager.service" >> $VNCSERVICE
echo " " >> $VNCSERVICE
echo "[Service]" >> $VNCSERVICE
echo "ExecStart=/usr/bin/x11vnc -xkb -noxrecord -noxfixes -noxdamage -display :0 -auth guess -rfbauth /etc/x11vnc.pass" >> $VNCSERVICE
echo "ExecStop=/usr/bin/killall x11vnc" >> $VNCSERVICE
echo "Restart=on-failure" >> $VNCSERVICE
echo "Restart-sec=2" >> $VNCSERVICE
echo " " >> $VNCSERVICE
echo "[Install]" >> $VNCSERVICE
echo "WantedBy=multi-user.target" >> $VNCSERVICE
echo " " >> $VNCSERVICE

systemctl daemon-reload;systemctl start x11vnc;systemctl enable x11vnc
}


PRINCIPAL()
{
echo "#############################################################" >>$LOGS
echo " ** INICIO $NOMELOG DO SISTEMA EM: `date`" >>$LOGS
echo "#############################################################" >>$LOGS

INSTALLVNC

echo "#############################################################" >>$LOGS
echo " * FIM $NOMELOG EM: `date`." >>$LOGS
echo "#############################################################" >>$LOGS
}

############################## MAIN

CONFGERAL
PRINCIPAL
