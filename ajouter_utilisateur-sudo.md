# Ajouter un utilisateur au groupe sudo sur Linux

Ce guide explique étape par étape comment donner les droits sudo à un utilisateur sous Linux. Cela permet à l'utilisateur d'exécuter des commandes avec les privilèges administrateur.

---

## Prérequis

- Avoir un accès root ou un utilisateur avec les droits sudo
- Connaître le nom exact de l'utilisateur concerné
- Fonctionne sur les distributions basées sur Debian/Ubuntu (ex : Ubuntu, Linux Mint)

---

## Étape 1 : Vérifier si l'utilisateur a déjà les droits sudo

Connectez-vous avec l'utilisateur concerné, puis exécutez :

```bash
sudo whoami
```

**Si le terminal répond :**

```
root
```

L'utilisateur a déjà les droits sudo.

**Sinon :**

```
<nom_utilisateur> is not in the sudoers file. This incident will be reported.
```

Passez à l'étape suivante.

---

## Étape 2 : Se connecter en tant que root

### Option 1 : Passer root via `su` (sur le terminal)

Si vous connaissez le mot de passe root :

```bash
su -
```

### Option 2 : Utiliser le mode recovery (sans le terminal)

1. Redémarrez votre machine
2. Appuyez sur `Shift` ou `Esc` pendant le démarrage pour ouvrir le menu **GRUB**
3. Choisissez **Advanced options for Ubuntu**
4. Sélectionnez un noyau en mode **(recovery mode)**
5. Dans le menu Recovery, choisissez **root - Drop to root shell prompt**
6. Montez le système en lecture/écriture :

```bash
mount -o remount,rw /
```

---

## Étape 3 : Ajouter l'utilisateur au groupe sudo

Remplacez `nom_utilisateur` par le nom de l'utilisateur concerné :

```bash
usermod -aG sudo nom_utilisateur
```

Puis redémarrez la machine :

```bash
reboot
```

---

## Étape 4 : Vérifier que ça fonctionne

Reconnectez-vous avec l'utilisateur concerné et tapez :

```bash
sudo whoami
```

Vous devez obtenir :

```
root
```

---

## Bonus : Liste des utilisateurs du groupe sudo

Pour voir tous les utilisateurs ayant les droits sudo :

```bash
getent group sudo
```

---
