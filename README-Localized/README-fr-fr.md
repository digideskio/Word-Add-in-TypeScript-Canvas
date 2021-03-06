# Exemple de complément Word pour les légendes d’images : chargement, modification et insertion d’images

**Table des matières**

* [Résumé](#summary)
* [Outils requis](#required-tools)
* [Installation de certificats](#how-to-install-certificates)
* [Comment configurer et exécuter l’application](#how-to-set-up-and-run-the-app)
* [Comment exécuter le complément dans Word 2016 pour Windows](#how-to-run-the-add-in-in-Word-2016-for-Windows)
* [FAQ](#faq)
* [Questions et commentaires](#questions-and-comments)
* [En savoir plus](#learn-more)


## Résumé

Cet exemple de complément Word vous explique comment procéder pour :

1. Créer un complément Word avec TypeScript.
2. Charger des images à partir du document vers le complément.
3. Modifier des images dans le complément à l’aide de l’API de canevas HTML et insérer des images dans un document Word.
4. Mettre en Å“uvre des commandes de complément qui lancent un complément à partir du ruban et exécutent un script à partir du ruban et d’un menu contextuel.
5. Utilisez la structure de l’interface utilisateur Office pour créer une expérience native semblable à Word pour votre complément.

![](/readme-images/Word-Add-in-TypeScript-Canvas.gif)

Définition - **Commande de complément** : extension de l’interface utilisateur Word qui vous permet de lancer le complément dans un volet Office ou d’exécuter un script à partir du ruban ou d’un menu contextuel.

Si vous souhaitez simplement voir cette opération, passez à la section [Configuration de Word 2016 pour Windows](#word-2016-for-windows-set-up) et utilisez ce [manifeste](https://github.com/OfficeDev/Word-Add-in-TypeScript-Canvas/blob/deploy2Azure/manifest-word-add-in-canvas.xml).

## Outils requis

Pour utiliser l’exemple de complément Word pour les légendes d’image, les éléments suivants sont requis.

* Word 2016 16.0.6326.0000 ou version ultérieure, ou tout autre client qui prend en charge l’API JavaScript de Word. Cet exemple effectue une vérification des conditions requises pour voir s’il est exécuté sur un hôte pris en charge par les API JavaScript.
* npm (https://www.npmjs.com/) pour installer les dépendances. Cet élément est fourni avec [NodeJS](https://nodejs.org/en/).
* (Windows) [Git Bash](http://www.git-scm.com/downloads).
* Clonez ce référentiel sur votre ordinateur local.

> Remarque : Word pour Mac 2016 ne prend pas en charge les commandes de complément à ce stade. Cet exemple peut s’exécuter sur Mac sans les commandes de complément.

## Installation de certificats

Vous aurez besoin d’un certificat pour exécuter cet exemple, car les commandes de complément exigent HTTPS. Étant donné que celles-ci ne disposent pas d’interface utilisateur, vous ne pouvez pas accepter les certificats non valides. Exécutez [./gen-cert.sh](#gen-cert.sh) pour créer le certificat, puis installez ca.crt dans votre magasin d’autorités de certification racines de confiance (Windows).

## Comment configurer et exécuter l’application

1. Installez le Gestionnaire des définitions TypeScript en saisissant ```npm install typings -g``` au niveau de la ligne de commande.
2. Installez les définitions TypeScript identifiées dans typings.json en exécutant ```typings install``` dans le répertoire racine du projet au niveau de la ligne de commande.
3. Installez les dépendances du projet identifiées dans package.json en exécutant ```npm install``` dans le répertoire racine du projet.
4. Installez l’élément Gulp ```npm install -g gulp```.
5. Copiez les fichiers Fabric et JQuery en exécutant ```gulp copy:libs```. (Windows) Si vous rencontrez un problème ici, vérifiez que *%APPDATA%\npm* se trouve dans votre variable de chemin d’accès.
6. Exécutez la tâche Gulp par défaut en exécutant ```gulp``` à partir du répertoire racine du projet. Si les définitions TypeScript ne sont pas mises à jour, vous recevrez une erreur ici.

À ce stade, vous avez déployé cet exemple de complément. Vous devez maintenant indiquer à Word où trouver le complément.

### Configuration de Word 2016 pour Windows

1. (Windows uniquement) Décompressez et exécutez cette [clé de Registre](https://github.com/OfficeDev/Office-Add-in-Commands-Samples/tree/master/Tools/AddInCommandsUndark) pour activer la fonctionnalité de commandes de complément. Ce paramètre est obligatoire lorsque les commandes de complément sont une **fonctionnalité d’aperçu**.
2. Créez un partage réseau ou [partagez un dossier sur le réseau](https://technet.microsoft.com/fr-fr/library/cc770880.aspx), puis placez-y le fichier manifeste [manifest-word-add-in-canvas.xml](manifest-word-add-in-canvas.xml).
3. Lancez Word et ouvrez un document.
4. Choisissez l’onglet **Fichier**, puis choisissez **Options**.
5. Choisissez **Centre de gestion de la confidentialité**, puis cliquez sur le bouton **Paramètres du Centre de gestion de la confidentialité**.
6. Choisissez **Catalogues de compléments approuvés**.
7. Dans la zone **URL du catalogue**, entrez le chemin réseau pour accéder au partage de dossier qui contient le fichier manifest-word-add-in-canvas.xml, puis choisissez **Ajouter un catalogue**.
8. Activez la case à cocher **Afficher dans le menu**, puis cliquez sur **OK**.
9. Un message vous informe que vos paramètres seront appliqués lors du prochain démarrage d’Office. Fermez et redémarrez Word.

## Comment exécuter le complément dans Word 2016 pour Windows

1. Ouvrez un document Word.
2. Dans l’onglet **Insertion** de Word 2016, choisissez **Mes compléments**.
3. Sélectionnez l’onglet **Dossier partagé**.
4. Choisissez **Complément pour les légendes d’image**, puis sélectionnez **Insérer**.
5. Si les commandes de complément sont prises en charge par votre version de Word, l’interface utilisateur vous informe que le complément a été chargé. Vous pouvez utiliser l’onglet **Complément pour les légendes d’image** pour charger le complément dans l’interface utilisateur et insérer une image dans le document. Vous pouvez également utiliser le menu contextuel pour insérer une image dans le document.
6. Si les commandes de complément ne sont pas prises en charge par votre version de Word, le complément est chargé dans un volet Office. Vous devez insérer une image dans le document Word pour utiliser la fonctionnalité du complément.
7. Sélectionnez une image dans le document Word et chargez-la dans le volet Office en sélectionnant *Charger l’image à partir du document*. Vous pouvez désormais insérer des légendes dans l’image. Sélectionnez *Insérer une image dans le document* pour placer l’image mise à jour dans le document Word. Le complément génère des descriptions d’espace réservé pour chaque légende.

## FAQ

* Les commandes de complément fonctionnent-elles sur Mac et sur iPad ? Non, elles ne fonctionnent pas sur Mac ou sur iPad lors de la publication de ce fichier readme.
* Pourquoi mon complément n’apparaît-il pas dans la fenêtre **Mes compléments** ? Votre manifeste de complément peut comporter une erreur. Je vous conseille de valider le manifeste par rapport au [schéma de manifeste](https://github.com/OfficeDev/Office-Add-in-Commands-Samples/tree/master/Tools/XSD).
* Pourquoi le fichier de fonction n’est-il pas appelé pour mes commandes de complément ? Les commandes de complément nécessitent HTTPS. Étant donné que les commandes de complément requièrent TLS et qu’elles ne comportent aucune interface utilisateur, vous ne pouvez pas voir s’il existe un problème de certificat. Si vous devez accepter un certificat non valide dans le volet Office, la commande de complément ne peut pas fonctionner.
* Pourquoi les commandes d’installation npm se bloquent-elles ? Elles ne sont probablement pas bloquées. Elle peuvent simplement être lentes sur Windows.

## Questions et commentaires

Nous serions ravis de connaître votre opinion sur l’exemple de complément pour les légendes d’image. Vous pouvez nous faire part de vos questions et suggestions dans la rubrique [Problèmes](https://github.com/OfficeDev/Word-Add-in-TypeScript-Canvas/issues) de ce référentiel.

Si vous avez des questions générales sur le développement de compléments, envoyez-les sur [Stack Overflow](http://stackoverflow.com/questions/tagged/Office365+API). Posez vos questions avec les balises [word-addins] et [API].

## En savoir plus

Voici des ressources supplémentaires pour vous aider à créer des compléments basés sur l’API JavaScript de Word :

* [Vue d’ensemble de la plateforme des compléments pour Office](https://msdn.microsoft.com/fr-fr/library/office/jj220082.aspx)
* [Compléments Word](https://github.com/OfficeDev/office-js-docs/blob/master/word/word-add-ins.md)
* [Présentation de la programmation JavaScript pour les compléments Word](https://github.com/OfficeDev/office-js-docs/blob/master/word/word-add-ins-programming-guide.md)
* [Explorateur d’extraits de code pour Word](http://officesnippetexplorer.azurewebsites.net/#/snippets/word)
* [Référence de l’API JavaScript pour les compléments Word](https://github.com/OfficeDev/office-js-docs/tree/master/word/word-add-ins-javascript-reference)
* [Exemples SillyStories](https://github.com/OfficeDev/Word-Add-in-SillyStories) - Découvrez comment charger des fichiers docx à partir d’un service et comment insérer ces fichiers dans un document Word ouvert.
* [Exemple d’authentification du complément Office sur le serveur pour Node.js](https://github.com/OfficeDev/Office-Add-in-Nodejs-ServerAuth) - Découvrez comment utiliser les fournisseurs Azure et Google OAuth pour l’authentification des utilisateurs de compléments.

## Copyright
Copyright (c) 2016 Microsoft. Tous droits réservés.
