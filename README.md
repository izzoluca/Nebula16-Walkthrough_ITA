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

<div class="highlight"><pre style="color:#f8f8f2;background-color:#272822;-moz-tab-size:4;-o-tab-size:4;tab-size:4"><code class="language-perl" data-lang="perl"><span style="color:#75715e">#!/usr/bin/env perl</span>

<span style="color:#66d9ef">use</span> CGI <span style="color:#e6db74">qw{param}</span>;

<span style="color:#66d9ef">print</span> <span style="color:#e6db74">"Content-type: text/html\n\n"</span>;

<span style="color:#66d9ef">sub</span> <span style="color:#a6e22e">login</span> {
  $username <span style="color:#f92672">=</span> $_[<span style="color:#ae81ff">0</span>];
  $password <span style="color:#f92672">=</span> $_[<span style="color:#ae81ff">1</span>];

  $username <span style="color:#f92672">=~</span> tr<span style="color:#e6db74">/a-z/</span>A<span style="color:#f92672">-</span>Z<span style="color:#f92672">/</span>; <span style="color:#75715e"># conver to uppercase</span>
  $username <span style="color:#f92672">=~</span> <span style="color:#e6db74">s/\s.*//</span>;        <span style="color:#75715e"># strip everything after a space</span>

  @output <span style="color:#f92672">=</span> <span style="color:#e6db74">`egrep "^$username" /home/flag16/userdb.txt 2&gt;&amp;1`</span>;
  <span style="color:#66d9ef">foreach</span> $line (@output) {
      ($usr, $pw) <span style="color:#f92672">=</span> split(<span style="color:#e6db74">/:/</span>, $line);
      <span style="color:#66d9ef">if</span>($pw <span style="color:#f92672">=~</span> $password) {
          <span style="color:#66d9ef">return</span> <span style="color:#ae81ff">1</span>;
      }
  }

  <span style="color:#66d9ef">return</span> <span style="color:#ae81ff">0</span>;
}

<span style="color:#66d9ef">sub</span> <span style="color:#a6e22e">htmlz</span> {
  <span style="color:#66d9ef">print</span>(<span style="color:#e6db74">"&lt;html&gt;&lt;head&gt;&lt;title&gt;Login resuls&lt;/title&gt;&lt;/head&gt;&lt;body&gt;"</span>);
  <span style="color:#66d9ef">if</span>($_[<span style="color:#ae81ff">0</span>] <span style="color:#f92672">==</span> <span style="color:#ae81ff">1</span>) {
      <span style="color:#66d9ef">print</span>(<span style="color:#e6db74">"Your login was accepted&lt;br/&gt;"</span>);
  } <span style="color:#66d9ef">else</span> {
      <span style="color:#66d9ef">print</span>(<span style="color:#e6db74">"Your login failed&lt;br/&gt;"</span>);
  }    
  <span style="color:#66d9ef">print</span>(<span style="color:#e6db74">"Would you like a cookie?&lt;br/&gt;&lt;br/&gt;&lt;/body&gt;&lt;/html&gt;\n"</span>);
}

htmlz(login(param(<span style="color:#e6db74">"username"</span>), param(<span style="color:#e6db74">"password"</span>)));</code><span class="copy-to-clipboard" title="Copy to clipboard"></span></pre></div>
