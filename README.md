# ssh-wincore2016
Script Pws basique pour l'installation de Openssh sur un Windows server core 2016

Prérequis indispensables : 

	- Accès à internet 

	- Résolution dns  

 

A faire : 

Se connecter en Administrateur LOCAL 

Copier le script dans C:\Users\Administrateur 

Exécuter le script PowerShell (script_openssh.ps1)

Le script ajoute de base le ssh sur le port 22 ( à modifier pour plus de sécurité) et donc ajoute aussi une règle par feu pour ce même port.  




Pourquoi l’ingénierie sociale fonctionnera toujours
Un message arrive entre deux réunions. Il est question d’un document à consulter. Pour l’ouvrir, le destinataire doit saisir un code sur une page Microsoft. Il vérifie l’adresse, se connecte et valide sa MFA. Rien, à première vue, ne sort de l’ordinaire.
Il vient pourtant peut-être de donner accès à son compte à un attaquant.
C’est ce qui rend le device code phishing déroutant : plusieurs des réflexes enseignés en sensibilisation peuvent avoir été respectés, sans que l’utilisateur comprenne ce qu’il est réellement en train d’autoriser.
On ne peut pas travailler en se méfiant de tout
Une journée de travail repose sur quantité de petits actes de confiance. On ouvre le fichier envoyé par un collègue. On répond à un fournisseur. On suit les indications du support pour débloquer une application. Vérifier systématiquement chacune de ces interactions rendrait le travail impraticable.
Les attaquants s’appuient sur ces habitudes. Une demande qui ressemble à la suite logique d’un dossier en cours n’attire pas la même attention qu’un message sans rapport avec notre activité. Elle arrive au bon moment, emploie les bons mots et demande un geste que l’on a déjà fait ailleurs.
La sensibilisation a toute sa place ici. Repérer une urgence fabriquée, vérifier une demande inhabituelle, refuser une notification MFA que l’on n’a pas déclenchée : ces réflexes s’apprennent. Il faut aussi savoir à qui signaler un doute, y compris quand on a déjà cliqué.
Mais les consignes doivent pouvoir s’appliquer dans une vraie journée de travail. Si un salarié est invité à prendre le temps de vérifier, puis rappelé à l’ordre parce que son dossier n’avance pas assez vite, il devra arbitrer. L’organisation ne peut pas lui demander de ralentir pour la sécurité tout en sanctionnant ce ralentissement.
Même lorsque ces conditions sont réunies, il reste la fatigue, les automatismes et les sollicitations bien préparées. Une formation peut réduire les erreurs. Elle ne permet pas de construire toute la sécurité de l’entreprise sur l’espoir que personne n’en commettra.
Une vraie page Microsoft peut faire partie du piège
À l’origine, le device code flow répond à un besoin légitime. Ce mécanisme OAuth permet notamment de connecter un équipement sur lequel la saisie est peu pratique. L’utilisateur entre un code dans le navigateur d’un autre appareil, puis s’authentifie. L’application qui a lancé la demande peut alors récupérer un jeton d’accès, ou access token, ainsi qu’un refresh token si les permissions le permettent, afin de renouveler cet accès. 1 — Documentation OAuth
Dans l’attaque, c’est l’attaquant qui lance la demande. Il fait ensuite parvenir le code à sa cible, par exemple dans une fausse invitation Teams. Celle-ci le saisit sur la véritable page Microsoft et termine sa connexion. Si les contrôles en place autorisent ce flux, les jetons sont délivrés au client de l’attaquant, pour les accès accordés. Microsoft a documenté ce procédé dans les campagnes de Storm-2372. 2 — Analyse de Microsoft
Figure 1. Échanges simplifiés lors d’un device code phishing. Si les contrôles autorisent le flux, les jetons sont délivrés au client qui a lancé la demande. [1]
L’utilisateur pense ouvrir un document. En réalité, il autorise une connexion lancée ailleurs. Sa MFA peut avoir été correctement effectuée, et le domaine qu’il a vérifié peut être le bon. Le contrôle de l’adresse reste utile ; il ne dit pas, à lui seul, à quoi servira l’authentification. 3 — Analyse de Huntress
Avec EvilTokens, cette technique prend aussi la forme d’une offre de Phishing-as-a-Service. Huntress décrit une plateforme qui automatise une partie de l’attaque et adapte les leurres au travail de la victime : proposition commerciale, demande de signature, formulaire à remplir. Le code est généré au moment où la personne ouvre la page, ce qui facilite l’enchaînement du scénario. [3]
La consigne « vérifiez le lien » doit donc être complétée. Lorsqu’un message demande de saisir un code de ce type, il faut pouvoir identifier l’application que l’on est en train de connecter et se rappeler si l’on a soi-même engagé cette opération. Si ce n’est pas clair, on s’arrête et on contacte le support par le canal habituel.
Un flux inutilisé devrait être bloqué
La question se pose aussi du côté de l’administration : le device code flow sert-il à quelque chose dans ce tenant ? Si aucun usage métier ne le justifie, le bloquer ferme cette voie d’accès.
Microsoft a fait évoluer les Security defaults dans ce sens. Depuis le 1er juillet 2026, les nouveaux tenants Entra ID bloquent le device code flow dans ce cadre. Cela ne dispense pas de vérifier les tenants existants, ni les environnements qui reposent sur leurs propres politiques de Conditional Access. 4 — Security defaults de Microsoft Entra ID
Avant de bloquer, il faut regarder les usages réels. Les journaux de connexion permettent de repérer où ce flux apparaît. Une politique de Conditional Access en mode Report-only aide ensuite à évaluer les conséquences d’un blocage. Microsoft recommande de restreindre le flux aussi largement que possible et de réserver les exceptions à des besoins documentés et sécurisés. 5 — Politique de blocage 6 — Suivi des flux d’authentification
Reste à passer de Report-only à On une fois les vérifications terminées. Tant que la politique est en observation, elle ne bloque rien. [5]
Figure 2. Démarche possible pour un tenant utilisant Conditional Access : examiner les usages, évaluer l’impact, puis activer le blocage avec les seules exceptions nécessaires. [5]
Ces exceptions demandent un suivi dans le temps. Une exclusion accordée pour un dépannage peut survivre longtemps au problème qui l’a justifiée. Il faut savoir qui l’utilise, pour quelle application, et qui se chargera de vérifier qu’elle reste nécessaire.
Limiter ce qu’une erreur permet de faire
Le même raisonnement s’applique aux autres accès. Une application oubliée ou des permissions devenues inutiles peuvent donner à un attaquant davantage de possibilités que prévu. Un compte conserve parfois les droits d’un ancien poste simplement parce que personne n’a pensé à les retirer. La réduction de la surface d’attaque passe aussi par ces revues régulières.
Les méthodes de MFA résistantes au phishing, comme les clés FIDO2 et les passkeys, renforcent la protection contre les fausses pages de connexion. Leur déploiement doit s’accompagner d’un contrôle des flux d’authentification et des permissions autorisées. [2]
Les processus métier ont également un rôle à jouer. Une demande de changement de coordonnées bancaires justifie un appel à un numéro déjà connu, même si elle provient du compte habituel du fournisseur. Pour certaines opérations sensibles, une seconde validation évite qu’un compte compromis suffise à tout autoriser.
La sensibilisation devient alors plus facile à appliquer : le salarié sait quand demander une vérification, et cette vérification est prévue dans le fonctionnement de l’entreprise.
Le SOC doit pouvoir comprendre ce qui a échappé aux contrôles
Des exceptions resteront parfois nécessaires. Le compte d’un partenaire pourra être compromis. Une demande convaincante sera validée malgré les précautions prises. L’entreprise doit pouvoir détecter ces situations et organiser la réponse. C’est le rôle du SOC, le Security Operations Center.
Dans un scénario de device code phishing, les journaux de connexion renseignent sur le flux utilisé. Un usage inhabituel peut être rapproché de l’enregistrement d’un nouvel appareil ou d’une activité anormale dans la messagerie. Pris séparément, ces événements ne suffisent pas à conclure. Leur enchaînement et le contexte du compte permettent d’évaluer ce qui s’est passé. [6][2]
Figure 3. Exemple de corrélation par un SOC. Les signaux présentés constituent des pistes d’analyse ; ils ne prouvent pas, à eux seuls, une compromission. [3][6]
Un déplacement, un changement de poste ou une intervention du support peuvent expliquer une partie de ces signaux. L’analyste doit pouvoir vérifier ces hypothèses, notamment en échangeant avec l’utilisateur. Un signalement tardif peut encore aider : « J’ai saisi un code après avoir reçu une invitation » apporte un élément que les logs ne racontent pas forcément.
Lorsque la compromission est probable, la réponse peut comprendre le blocage du compte, la révocation des refresh tokens et des sessions, puis la recherche des données consultées et des modifications effectuées. Changer uniquement le mot de passe ne suffit pas à traiter tous les accès déjà obtenus. Il faut également tenir compte de la façon dont chaque application gère ses jetons et ses sessions, car les révocations n’ont pas toutes un effet immédiat. 7 — Révocation des accès dans Entra ID
Pour intervenir à temps, le SOC a besoin de journaux exploitables, de détections adaptées et d’une procédure qui précise les actions à engager. Il ne verra pas nécessairement tout. Il doit en revanche disposer des moyens d’enquêter et de contenir un incident lorsqu’un indice apparaît.
L’ingénierie sociale continuera de fonctionner parce qu’elle s’insère dans des échanges dont nous avons besoin pour travailler. La sensibilisation reste indispensable, à condition de lui donner des suites concrètes dans les outils et les procédures. Le jour où quelqu’un saisira ce code entre deux réunions, l’entreprise devra pouvoir compter sur les protections et les moyens de réponse qu’elle aura préparés.