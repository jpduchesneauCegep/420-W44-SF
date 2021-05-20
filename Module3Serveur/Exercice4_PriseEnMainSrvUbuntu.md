# Exercice 4 - Prise en main de votre serveur

Évaluation : formative
Durée estimée : 1 heures
Système d'exploitation : Ubuntu 20.04 Lts

 

## Prise en main de votre serveur


 Quand vous vous connectez à votre serveur, certaines [informations](Images/Connexion.png) importantes vous sont fournies. l'adresse IP est l'une de plus importante. Notez là.

Lors de l'installation, vous avez coché oui à l'installation d'un serveur SSH.
<details> 
<blockquote>Celui-ci vas permette aux utilisateurs d'accéder au système à distance, en rentrant leur login et leur mot de passe (ou avec un mécanisme de clefs).

Cela signifie aussi qu’un pirate peut essayer d’avoir un compte sur le système (pour accéder à des fichiers sur le système ou pour utiliser le système comme une passerelle pour attaquer d’autres systèmes) en essayant plein de mots de passes différents pour un même login (il peut le faire de manière automatique en s’aidant d’un dictionnaire électronique). On appelle ça une attaque en force brute. Source : Les citations sur SSH proviennent de : Formation Debian GNU/Linux ECP, janvier 2013, document PDF.

Il y a donc trois contraintes majeures pour garder un système sécurisé après avoir
installé un serveur SSH :
– avoir un serveur SSH à jour au niveau de la sécurité, ce qui doit être le cas si vous
faites consciencieusement les mises à jour de sécurité en suivant la procédure;
– que les mots de passes de TOUS les utilisateurs soient suffisamment complexes
pour résister à une attaque en force brute ;
– surveiller les connexions en lisant régulièrement le fichier de log /var/log/
auth.log.</blockquote>
Source : Les citations sur SSH proviennent de : Formation Debian GNU/Linux ECP, janvier 2013, document PDF.

<i>Nous aborderons la notion de mise de sécurité et la lecture des logs plus .tard.</i>
</details>
## L’établissement d’une connexion SSH

#### Le système de clefs de SSH
<details>
<blockquote>SSH utilise la cryptographie asymétrique RSA ou DSA. En cryptographie asymétrique, chaque personne dispose d’un couple de clef : une clé publique et une clef privée. La clé publique peut être librement publiée tandis que la clef privée doit rester secrète. La connaissance de la clef publique ne permet pas d’en déduire la clé privée.
</blockquote>
Un serveur SSH dispose d’un couple de clefs RSA stocké dans le répertoire /etc/
ssh/ et généré lors de l’installation du serveur. Le fichier ssh_host_rsa_key
contient la clef privée et a les permissions 600. Le fichier ssh_host_rsa_key.pub contient la clef publique et a les permissions 644.
</details>
### Voici les étapes de l’établissement d’une connexion SSH :

1. Le serveur envoie sa clef publique au client. Celui-ci vérifie qu’il s’agit bien de la clef du serveur, s’il l’a déjà reçue lors d’une connexion précédente.
2. Le client génère une clef secrète et l’envoie au serveur, en chiffrant l’échange avec la clef publique du serveur (chiffrement asymétrique). Le serveur déchiffre cette clef secrète en utilisant sa clé privée, ce qui prouve qu’il est bien le vrai serveur.
3. Pour le prouver au client, il chiffre un message standard avec la clef secrète et l’envoie au client. Si le client retrouve le message standard en utilisant la clef secrète, il a la preuve que le serveur est bien le vrai serveur. 
4. Une fois la clef secrète échangée, le client et le serveur peuvent alors établir un canal sécurisé grâce à la clef secrète commune (chiffrement symétrique).
5. Une fois que le canal sécurisé est en place, le client va pouvoir envoyer au serveur le login et le mot de passe de l’utilisateur pour vérification. La canal sécurisé reste en place jusqu’à ce que l’utilisateur se déconnecte.




### Connexion depuis votre poste de développeur

Utilisez votre machine ubuntu client (poste de developpeur) pour établir une connection avec votre serveur.
 
- Dans un terminal sur le poste client taper la commande :
```bash
$ssh {adresse ip du serveur}  [-l votre nom d'usager sur le serveur] [- port]
```
- Remarquez l'échange de la clé lors de la première connexion.
- Vérifier par quelques commandes que vous êtes bien sur le serveur : 
```bash
$uname
$ip -a
$netstat -tap
$df
$pwd
$top
```
- Taper les mêmes commandes dans un autres terminal, mais cette fois sur votre poste client. 
- Au besoin, vérifier le manuel de chaque commande (man).
 
####Vérifier les logs du serveur

- Tapez la commande suivante sur votre  poste client et sur le serveur pour observer le connexions.

```bash
$tail /var/log/auth.log
```
### Vérifier les droits sur des fichiers et des répertoires

Pour comprendre les droits : [https://doc.ubuntu-fr.org/droits](https://doc.ubuntu-fr.org/droits)
- Vérifier les droits sur le fichier des logs

```bash
$ls -l /var/log/auth.log
```
- Garder l'information pour la comparer plus tard. Notez les droits de cette façon :

    **exemple appliqué sur /var/log/auth.log**

     -rw-r----- syslog adm

   - ici on a fichier, première lette (-)
   - lire, écrire, null pour le propriétaire
   - lecture seulement pour le groupe
   - et aucun droit pour les autres
   - Le pripriétaire est syslog et le groupe adm
   

- Comparer les droits avec ceux des fichiers suivants :

```bash
$ls -l /etc/passwd
$ls -l /etc/shadow
$ls -l /home/{votre usager}
```
Vous pouvez visualiser mes résultats [ici](Images/droit.png).