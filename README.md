# Nebula16-Walkthrough_ITA
<p> Nebula è una macchina virtuale vulnerabile che presenta punti deboli e vulnerabilità in un sistema Linux. </p>

Nebula accompagna il partecipante attraverso una serie di punti deboli e vulnerabilità comuni (e meno comuni) in Linux. Diamo uno sguardo più nel dettaglio:

<ul>
<li>File SUID</li>
<li>Autorizzazioni</li>
<li>Race conditions</li>
<li>Meta-variabili della Shell</li>
<li>Punti deboli del "$PATH"</li>
<li>Debolezze del linguaggio di scripting</li>
<li>Errori di compilazione binaria</li>
</ul>
<p>Alla fine di Nebula, l'utente avrà una comprensione abbastanza approfondita degli attacchi locali contro i sistemi Linux e uno sguardo superficiale ad alcuni degli attacchi remoti possibili.</p>


<h2> Download Nebula </h2>
<p> Il download è disponibile <a href="https://exploit.education/downloads/"> qui </a>  </p>

<h2> Per iniziare </h2>
<p> Accedi alla macchina virtuale come nome utente "levelXX" con una password di "levelXX" (senza virgolette), dove XX è il numero del livello. Alcuni livelli possono essere eseguiti in modo puramente remoto. </p>

<h2> Ottenere i permessi di Root </h2>
<p> Nel caso in cui sia necessario l'accesso root per modificare cose (come mappature dei tasti, ecc.), Puoi fare quanto segue: <br>
Utente = "nebula" & Password = "nebula" seguita da "sudo -s" con la password "nebula". Avrai quindi i privilegi di root per cambiare ciò che deve essere cambiato. </p>


<h1> LEVEL 16 </h1>
<p> C'è uno script perl in esecuzione sulla porta 1616. <br>
Per fare questo livello, accedi come l'account level16 con la password level16. I file per questo livello possono essere trovati in "/home/flag16". </p>

<h2> Codice Sorgente </h2>
<p>
<code>

#!/usr/bin/env perl

use CGI qw{param};

print "Content-type: text/html\n\n";

sub login {
  $username = $_[0];
  $password = $_[1];

  $username =~ tr/a-z/A-Z/; # conver to uppercase
  $username =~ s/\s.*//;        # strip everything after a space

  @output = `egrep "^$username" /home/flag16/userdb.txt 2>&1`;
  foreach $line (@output) {
      ($usr, $pw) = split(/:/, $line);
  

      if($pw =~ $password) {
          return 1;
      }
  }

  return 0;
}

sub htmlz {
  print("<html><head><title>Login resuls</title></head><body>");
  if($_[0] == 1) {
      print("Your login was accepted<br/>");
  } else {
      print("Your login failed<br/>");
  }    
  print("Would you like a cookie?<br/><br/></body></html>\n");
}

htmlz(login(param("username"), param("password")));

</code>
</p>
