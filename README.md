# Guide d'installation, de désinstallation et d'utilisation de WildFly
## Table de matiere
- [Ubuntu](#ubuntu)
    - [Installation sous ubuntu](#installation-sous-ubuntu)
    - [Désinstallation sous ubuntu](#désinstallation-sous-ubuntu)
- [Windows 11](#windows-11)
    - [Installation sous windows](#installation-sous-windows)
    - [Désinstallation sous windows](#désinstallation-sous-windows)
- [Utilisation](#utilisation)
## Ubuntu
### Installation sous ubuntu

- Installer Java :
WildFly nécessite Java pour fonctionner. Installez OpenJDK :
```bash
    sudo apt update
    sudo apt install openjdk-17-jdk
```

- Télécharger WildFly :
Téléchargez la dernière version de WildFly :
```bash
    wget https://download.jboss.org/wildfly/26.1.3.Final/wildfly-26.1.3.Final.tar.gz
```

- Extraire l'archive :
```bash
    tar xzf wildfly-26.1.3.Final.tar.gz
```

- Déplacer WildFly vers le répertoire /opt :
```bash
    sudo mv wildfly-26.1.3.Final /opt/wildfly
```

- Créer un utilisateur pour WildFly :
```bash
    sudo useradd -r -s /sbin/nologin wildfly
    sudo chown -R wildfly:wildfly /opt/wildfly
```

- Créer un fichier de service Systemd :
```bash
    sudo nano /etc/systemd/system/wildfly.service
```

- Ajoutez le contenu suivant :
```ini  
    [Unit]
    Description=WildFly Application Server
    After=network.target

    [Service]
    User=wildfly
    Group=wildfly
    ExecStart=/opt/wildfly/bin/standalone.sh -b=0.0.0.0
    ExecStop=/opt/wildfly/bin/jboss-cli.sh --connect command=:shutdown
    Restart=on-failure
    LimitNOFILE=102642

    [Install]
    WantedBy=multi-user.target
```

- Recharger Systemd et démarrer WildFly :
```bash
    sudo systemctl daemon-reload
    sudo systemctl start wildfly
    sudo systemctl enable wildfly
```

- Configurer l'utilisateur d'administration :
```bash
    /opt/wildfly/bin/add-user.sh
```

- Vérifier l'installation :
    - Ouvrez votre navigateur et allez sur http://localhost:8080.

### Désinstallation sous ubuntu

- Arrêter le service WildFly :
```bash
    sudo systemctl stop wildfly
```

- Désactiver le service WildFly :
```bash
    sudo systemctl disable wildfly
```

- Supprimer les fichiers WildFly :
```bash
    sudo rm -rf /opt/wildfly
```

- Supprimer le fichier de service Systemd :
```bash
    sudo rm /etc/systemd/system/wildfly.service
```

- Recharger Systemd :
```bash
    sudo systemctl daemon-reload
```

## Windows 11
### Installation sous windows

- Installer Java :
    - Téléchargez et installez Java depuis le [site Oracle](https://www.oracle.com/java/technologies/downloads/?er=221886).

- Télécharger WildFly :
    - Téléchargez la dernière distribution binaire de WildFly depuis le [site officiel]().

- Extraire l'archive :
    - Extrayez le fichier ZIP téléchargé dans un répertoire souhaité (par exemple, `C:\WildFly`).

- Définir les variables d'environnement :
    - Ouvrez le Menu Démarrer et cherchez `Variables d'environnement`.
    - Cliquez sur `Modifier`.
    - Dans la fenêtre Propriétés Système, cliquez sur `Variables d'environnement`.
    - Sous `Variables système`, cliquez sur `Nouveau` et créez une nouvelle variable `JBOSS_HOME` avec la valeur définie sur le répertoire WildFly (par exemple, `C:\WildFly`).
    - De même, créez ou mettez à jour la variable `JAVA_HOME` pour pointer vers le répertoire d'installation de Java (par exemple, `C:\Program Files\Java\jdk-17.0.10`).

- Démarrer WildFly :
    - Accédez au répertoire `C:\WildFly\bin`.
    - Double-cliquez sur `standalone.bat`.

- Configurer l'utilisateur d'administration :
    - Accédez au répertoire `C:\WildFly\bin`.
    - Double-cliquez sur `add-user.bat` et suivez les instructions pour ajouter un utilisateur d'administration.

- Vérifier l'installation :
Ouvrez votre navigateur et allez sur http://localhost:8080.

### Désinstallation sous windows

- Arrêter WildFly :
    - Accédez au répertoire `C:\WildFly\bin`.
    - Double-cliquez sur `jboss-cli.bat` et tapez `connect` puis :`shutdown` pour arrêter le serveur.

- Supprimer le répertoire WildFly :
    - Supprimez le répertoire `C:\WildFly`.

- Supprimer les variables d'environnement :
    - Ouvrez le Menu Démarrer et cherchez `Variables d'environnement`.
    - Cliquez sur `Modifier`.
    - Dans la fenêtre Propriétés Système, cliquez sur "Variables d'environnement".
    - Sous `Variables système`, trouvez et supprimez la variable `JBOSS_HOME`.
    - Optionnellement, si elle n'est pas utilisée par d'autres programmes, supprimez la variable `JAVA_HOME`.

## Utilisation

- Déployer une application web :
    - Copiez le fichier `WAR` de votre application dans le répertoire `standalone/deployments` de votre répertoire d'installation WildFly.
    - WildFly déploiera automatiquement l'application.

- Accéder à la console d'administration :
    - Ouvrez votre navigateur et allez sur http://localhost:9990.
    - Connectez-vous en utilisant les identifiants créés lors de la configuration de l'utilisateur d'administration.

- Surveiller et gérer les applications :
    - Utilisez la console d'administration WildFly pour démarrer, arrêter et gérer les applications déployées, configurer le serveur, et surveiller les performances.