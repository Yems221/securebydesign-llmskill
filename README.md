ENGLISH 
-----
-----
FRENCH 
------
------
SPANISH

EN
SecureByDesign
One skill. Twenty‑five security checks. Works with any AI assistant.

https://img.shields.io/badge/version-1.1.0-blue.svg
https://img.shields.io/badge/License-MIT-yellow.svg
https://img.shields.io/badge/website-securebydesign.saccessa.com-green

What this is
A skill is a text file you give to an AI—Claude, ChatGPT, Cursor, any coding assistant. It tells the AI how to behave.

SecureByDesign is that text file. It contains 25 security checks. When you add it to your AI, the AI applies those checks to every line of code it writes and every recommendation it makes.

The skill was released in March 2026. It is free. It is open source (MIT). It does one thing: it makes the AI check your code against a fixed list of security rules.

What the skill actually does
The skill is 25 controls, organised in five layers. Each control says: here is a common security mistake, here is what to do instead, here is the standard that requires it.

Layer 1 – Input and output

SBD‑01: validate every input against an explicit list of allowed values

SBD‑02: keep user input separate from system instructions when talking to an LLM

SBD‑03: set the correct security headers and encode output for its destination

Layer 2 – Identity and access

SBD‑04: use Argon2id or bcrypt for passwords, enforce MFA for privileged accounts, rate‑limit login attempts

SBD‑05: check ownership on every request—user A cannot see user B's data

SBD‑06: give each component the minimum permissions it needs

Layer 3 – Data protection

SBD‑07: no credentials in source code. ever.

SBD‑08: use modern crypto (AES‑256‑GCM, TLS 1.3). not MD5, not SHA‑1, not Math.random() for tokens.

SBD‑09: collect only what you need. do not log sensitive data.

Layer 4 – Resilience and monitoring

SBD‑10: log security events with enough context to investigate an incident

SBD‑11: set rate limits on public endpoints and hard caps on LLM token usage

SBD‑12: block requests to private IP ranges (SSRF protection)

SBD‑13: show generic errors to users, log details server‑side

Layer 5 – Supply chain and architecture

SBD‑14: pin dependencies, scan for vulnerabilities, review AI‑suggested packages

SBD‑15: pin CI/CD actions to commit SHAs, not version tags

SBD‑16: verify model checksums before loading third‑party LLMs

SBD‑17: test system prompts against extraction attempts

SBD‑18: filter vector database results by user—user A cannot retrieve user B's documents

SBD‑19: validate LLM output before passing it to a database, a shell, or a browser

SBD‑20: set CORS to explicit origins, never * on authenticated endpoints

SBD‑21: fail secure—deny access if the permission check errors

SBD‑22: make security review part of your Definition of Done

SBD‑23: keep an inventory of every service, API, and integration

SBD‑24: define alert thresholds before an incident happens

SBD‑25: identify applicable privacy regulations at the start of the project

Each control has:

a short description of the problem

an example of code that fails the check

an example of code that passes

the standards it maps to (OWASP, NIST, ISO, CIS)

The skill also includes:

a version check: the AI tells you if a newer version exists

language detection: responds in English, French, or Spanish based on your messages

three tiers: LOW (personal projects), STANDARD (business apps), REGULATED (finance, health, government)

anti‑hallucination rules: the AI only claims something is secure if it can show you working code for your stack

conflict resolution: rules for when two controls pull in different directions (e.g. data minimisation vs security logging)

What this skill is not
SecureByDesign is not a magic wand.

It does not:

scan your running application – it reviews code and architecture, but it does not attack your system

enforce anything in production – it tells you what to do; you still have to implement it

replace a penetration test – for regulated industries, you still need a formal audit

fix your code automatically – it flags problems and shows fixes, but you (or your AI) write the corrected code

check every possible vulnerability – it covers 25 common patterns, not every CWE

The skill is honest about its limits. Every report ends with this:

This analysis covers known vulnerability patterns in the code and architecture provided. It does not replace penetration testing, formal threat modeling, or a certified security audit for systems handling sensitive or regulated data.

Who it is for
developers who want to catch mistakes before they ship

teams that need a consistent security baseline across projects

non‑profits and small organisations with limited security resources

anyone who uses an AI to write code and wants to avoid the most common pitfalls

You do not need to be a security expert. The skill does the checking.

Basic security practices the skill flags
When you are moving fast, it is easy to miss the basics. The skill catches them.

What the user writes	What the skill flags
password = "admin123" (even if hashed)	weak password – requires minimum length, complexity, and checks against common dictionaries
api_key = "sk-abc123..." in the code	hardcoded secret – must be moved to environment variables
SELECT * FROM users WHERE id = + user_id	SQL injection risk – must use parameterised queries
user input placed directly in the system prompt	prompt injection risk – requires role‑separated messages
JWT with no exp or alg: none	broken authentication – must set expiration and verify algorithm
Access-Control-Allow-Origin: * on an authenticated endpoint	CORS misconfiguration – requires explicit origin list
stack trace shown to the user	information disclosure – generic error messages only
no rate limiting on /login	account takeover risk – limits per IP and per account required
The skill does not just flag these. It explains why they matter and shows the fix.

How to use it
1. Download the skill
Go to the GitHub repository and save SKILL.md to your computer.
https://github.com/securebydesign/skill/blob/main/SKILL.md

2. Add it to your AI
Every tool has a different place for instructions. The website has detailed guides for each one.

Claude.ai – create a Project, paste into Project instructions
ChatGPT – create a custom GPT, paste into Instructions
Claude Code – create CLAUDE.md in your project root, paste the skill
Cursor – create a rule in .cursor/rules/, paste the skill
Windsurf – Customizations → Rules → Workspace Rule, paste the skill
Cline – create .clinerules/securebydesign.md, paste the skill
Ollama – create a Modelfile, add the skill to the SYSTEM section
Any API – load SKILL.md at startup, pass it as the first system message

For step‑by‑step guides: securebydesign.saccessa.com

3. Keep working
From that point, every answer your AI gives will be checked against the 25 controls.

What you see when it finds something
When the AI spots a problem, it tells you directly:

text
❌ SBD-07: Secrets in source code
   → API key at line 12 of config.py
   → Risk: anyone with access to the repo can use this key
   → Fix: move to environment variable
   → Code:
        import os
        api_key = os.environ["SERVICE_KEY"]   # correct
        # never: api_key = "sk-abc123..."
If the AI is uncertain because your stack is not well represented in its training, it says so:

I have limited knowledge of [X]. The following is based on general security principles. Verify specifics against [X] documentation.

Repository
This repository contains three files:

text
securebydesign/skill/
├── SKILL.md        # the skill itself – load this into your AI
├── README.md       # this file
└── LICENSE         # MIT license – use freely, modify, share
License and support
The skill is MIT licensed. Use it in any project. Modify it. Share it.

If you want to support the work:
➡️ GitHub star – helps others find it
➡️ Financial contribution – funds research and maintenance

Supporters at $500 or more are listed in the changelog and on the website. No obligation. The MIT license is unconditional.

Learn more
Website with installation guides and control explanations:
securebydesign.saccessa.com

GitHub repository:
https://github.com/Yems221/securebydesign-llmskill

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
FR

SecureByDesign
Un skill. Vingt‑cinq vérifications de sécurité. Fonctionne avec n'importe quel assistant IA.

https://img.shields.io/badge/version-1.1.0-blue.svg
https://img.shields.io/badge/Licence-MIT-yellow.svg
https://img.shields.io/badge/site%2520web-securebydesign.saccessa.com-green

Ce que c'est
Un skill est un fichier texte que vous donnez à une IA – Claude, ChatGPT, Cursor, n'importe quel assistant de code. Il dit à l'IA comment se comporter.

SecureByDesign est ce fichier texte. Il contient 25 vérifications de sécurité. Quand vous l'ajoutez à votre IA, l'IA applique ces vérifications à chaque ligne de code qu'elle écrit et à chaque recommandation qu'elle fait.

Le skill a été publié en mars 2026. Il est gratuit. Il est open source (MIT). Il fait une chose : il fait vérifier votre code par l'IA contre une liste fixe de règles de sécurité.

Ce que le skill fait exactement
Le skill comprend 25 contrôles, organisés en cinq couches. Chaque contrôle dit : voici une erreur de sécurité courante, voici ce qu'il faut faire à la place, voici la norme qui l'exige.

Couche 1 – Entrées et sorties

SBD‑01 : valider chaque entrée contre une liste explicite de valeurs autorisées

SBD‑02 : garder les entrées utilisateur séparées des instructions système quand on parle à un LLM

SBD‑03 : définir les en‑têtes de sécurité corrects et encoder les sorties pour leur destination

Couche 2 – Identité et accès

SBD‑04 : utiliser Argon2id ou bcrypt pour les mots de passe, imposer la MFA pour les comptes privilégiés, limiter les tentatives de connexion

SBD‑05 : vérifier la propriété sur chaque requête – l'utilisateur A ne peut pas voir les données de l'utilisateur B

SBD‑06 : donner à chaque composant les permissions minimales dont il a besoin

Couche 3 – Protection des données

SBD‑07 : aucun identifiant dans le code source. jamais.

SBD‑08 : utiliser de la crypto moderne (AES‑256‑GCM, TLS 1.3). pas de MD5, pas de SHA‑1, pas de Math.random() pour les jetons.

SBD‑09 : ne collecter que ce dont vous avez besoin. ne pas journaliser les données sensibles.

Couche 4 – Résilience et surveillance

SBD‑10 : journaliser les événements de sécurité avec assez de contexte pour enquêter sur un incident

SBD‑11 : fixer des limites de débit sur les points d'accès publics et des plafonds stricts sur l'utilisation des jetons LLM

SBD‑12 : bloquer les requêtes vers les plages IP privées (protection SSRF)

SBD‑13 : montrer des erreurs génériques aux utilisateurs, journaliser les détails côté serveur

Couche 5 – Chaîne d'approvisionnement et architecture

SBD‑14 : épingler les dépendances, analyser les vulnérabilités, vérifier les paquets suggérés par l'IA

SBD‑15 : épingler les actions CI/CD sur des SHA de commit, pas sur des tags de version

SBD‑16 : vérifier les sommes de contrôle des modèles avant de charger des LLM tiers

SBD‑17 : tester les prompts système contre les tentatives d'extraction

SBD‑18 : filtrer les résultats des bases vectorielles par utilisateur – l'utilisateur A ne peut pas récupérer les documents de l'utilisateur B

SBD‑19 : valider les sorties LLM avant de les passer à une base de données, un shell ou un navigateur

SBD‑20 : définir CORS sur des origines explicites, jamais * sur des points d'accès authentifiés

SBD‑21 : échouer de manière sécurisée – refuser l'accès si la vérification des permissions échoue

SBD‑22 : faire de la revue de sécurité une partie de votre Definition of Done

SBD‑23 : tenir un inventaire de chaque service, API et intégration

SBD‑24 : définir des seuils d'alerte avant qu'un incident ne se produise

SBD‑25 : identifier les réglementations applicables en matière de confidentialité au début du projet

Chaque contrôle a :

une courte description du problème

un exemple de code qui échoue à la vérification

un exemple de code qui réussit

les normes auxquelles il se rapporte (OWASP, NIST, ISO, CIS)

Le skill comprend aussi :

une vérification de version : l'IA vous dit si une version plus récente existe

la détection de la langue : répond en anglais, français ou espagnol selon vos messages

trois niveaux : FAIBLE (projets personnels), STANDARD (applications métier), RÉGLEMENTÉ (finance, santé, gouvernement)

des règles anti‑hallucination : l'IA ne prétend qu'une chose est sécurisée que si elle peut vous montrer du code qui fonctionne pour votre pile technique

une résolution des conflits : des règles pour quand deux contrôles tirent dans des directions différentes (par exemple, minimisation des données vs journalisation de sécurité)

Ce que ce skill n'est pas
SecureByDesign n'est pas une baguette magique.

Il ne :

scan pas votre application en cours d'exécution – il examine le code et l'architecture, mais il n'attaque pas votre système

fait pas respecter quoi que ce soit en production – il vous dit ce qu'il faut faire ; c'est à vous de l'implémenter

remplace pas un test d'intrusion – pour les secteurs réglementés, vous avez toujours besoin d'un audit formel

corrige pas votre code automatiquement – il signale les problèmes et montre les corrections, mais vous (ou votre IA) écrivez le code corrigé

vérifie pas toutes les vulnérabilités possibles – il couvre 25 schémas courants, pas toutes les CWE

Le skill est honnête sur ses limites. Chaque rapport se termine par :

Cette analyse couvre les schémas de vulnérabilité connus dans le code et l'architecture fournis. Elle ne remplace pas les tests d'intrusion, la modélisation formelle des menaces ou un audit de sécurité certifié pour les systèmes traitant des données sensibles ou réglementées.

À qui cela s'adresse
aux développeurs qui veulent attraper les erreurs avant de livrer

aux équipes qui ont besoin d'une base de sécurité cohérente entre les projets

aux associations et petites organisations avec peu de ressources en sécurité

à toute personne qui utilise une IA pour écrire du code et veut éviter les pièges les plus courants

Vous n'avez pas besoin d'être un expert en sécurité. Le skill fait les vérifications.

Pratiques de sécurité de base que le skill signale
Quand on va vite, il est facile d'oublier les bases. Le skill les attrape.

Ce que l'utilisateur écrit	Ce que le skill signale
password = "admin123" (même si haché)	mot de passe faible – nécessite une longueur minimale, de la complexité et une vérification par rapport aux dictionnaires courants
api_key = "sk-abc123..." dans le code	secret codé en dur – doit être déplacé dans des variables d'environnement
SELECT * FROM users WHERE id = + user_id	risque d'injection SQL – doit utiliser des requêtes paramétrées
entrée utilisateur placée directement dans le prompt système	risque d'injection de prompt – nécessite des messages séparés par rôle
JWT sans exp ou avec alg: none	authentification cassée – doit définir une expiration et vérifier l'algorithme
Access-Control-Allow-Origin: * sur un point d'accès authentifié	mauvaise configuration CORS – nécessite une liste explicite d'origines
trace d'appel montrée à l'utilisateur	divulgation d'informations – messages d'erreur génériques uniquement
pas de limitation de débit sur /login	risque de prise de contrôle de compte – limites par IP et par compte exigées
Le skill ne se contente pas de signaler. Il explique pourquoi c'est important et montre la correction.

Comment l'utiliser
1. Télécharger le skill
Allez sur le dépôt GitHub et enregistrez SKILL.md sur votre ordinateur.
https://github.com/securebydesign/skill/blob/main/SKILL.md

2. L'ajouter à votre IA
Chaque outil a un endroit différent pour les instructions. Le site web a des guides détaillés pour chacun.

Claude.ai – créer un Projet, coller dans les instructions du Projet
ChatGPT – créer un GPT personnalisé, coller dans les Instructions
Claude Code – créer CLAUDE.md à la racine du projet, coller le skill
Cursor – créer une règle dans .cursor/rules/, coller le skill
Windsurf – Personnalisations → Règles → Règle d'espace de travail, coller le skill
Cline – créer .clinerules/securebydesign.md, coller le skill
Ollama – créer un Modelfile, ajouter le skill dans la section SYSTEM
API – charger SKILL.md au démarrage, le passer comme premier message système

Pour les guides pas à pas : securebydesign.saccessa.com

3. Continuer à travailler
À partir de là, chaque réponse de votre IA sera vérifiée par rapport aux 25 contrôles.

Ce que vous voyez quand il trouve quelque chose
Quand l'IA repère un problème, elle vous le dit directement :

text
❌ SBD-07 : Secret dans le code source
   → Clé API à la ligne 12 de config.py
   → Risque : toute personne ayant accès au dépôt peut utiliser cette clé
   → Correction : déplacer dans une variable d'environnement
   → Code :
        import os
        api_key = os.environ["SERVICE_KEY"]   # correct
        # jamais : api_key = "sk-abc123..."
Si l'IA n'est pas sûre parce que votre pile technique est mal représentée dans ses données d'apprentissage, elle le dit :

Ma connaissance de [X] est limitée. Ce qui suit est basé sur des principes généraux de sécurité. Vérifiez les spécificités dans la documentation de [X].

Dépôt
Ce dépôt contient trois fichiers :

text
securebydesign/skill/
├── SKILL.md        # le skill lui-même – à charger dans votre IA
├── README.md       # ce fichier
└── LICENSE         # licence MIT – utilisez librement, modifiez, partagez
Licence et soutien
Le skill est sous licence MIT. Utilisez‑le dans n'importe quel projet. Modifiez‑le. Partagez‑le.

Si vous voulez soutenir le travail :
➡️ Étoile GitHub – aide d'autres à le trouver
➡️ Contribution financière – finance la recherche et la maintenance

Les soutiens à partir de 500 $ sont listés dans le journal des modifications et sur le site web. Aucune obligation. La licence MIT est inconditionnelle.

En savoir plus
Site web avec guides d'installation et explications des contrôles :
securebydesign.saccessa.com

Dépôt GitHub :
https://github.com/Yems221/securebydesign-llmskill

-------------------------------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------------------------------
ES
SecureByDesign
Un skill. Veinticinco comprobaciones de seguridad. Funciona con cualquier asistente de IA.

https://img.shields.io/badge/version-1.1.0-blue.svg
https://img.shields.io/badge/Licencia-MIT-yellow.svg
https://img.shields.io/badge/sitio%2520web-securebydesign.saccessa.com-green

Qué es esto
Un skill es un archivo de texto que le das a una IA – Claude, ChatGPT, Cursor, cualquier asistente de código. Le dice a la IA cómo comportarse.

SecureByDesign es ese archivo de texto. Contiene 25 comprobaciones de seguridad. Cuando lo añades a tu IA, la IA aplica esas comprobaciones a cada línea de código que escribe y a cada recomendación que hace.

El skill se publicó en marzo de 2026. Es gratuito. Es de código abierto (MIT). Hace una cosa: hace que la IA revise tu código contra una lista fija de reglas de seguridad.

Qué hace exactamente el skill
El skill tiene 25 controles, organizados en cinco capas. Cada control dice: aquí hay un error de seguridad común, aquí está lo que hay que hacer en su lugar, aquí está el estándar que lo exige.

Capa 1 – Entradas y salidas

SBD‑01: validar cada entrada contra una lista explícita de valores permitidos

SBD‑02: mantener la entrada del usuario separada de las instrucciones del sistema al hablar con un LLM

SBD‑03: establecer las cabeceras de seguridad correctas y codificar la salida para su destino

Capa 2 – Identidad y acceso

SBD‑04: usar Argon2id o bcrypt para contraseñas, exigir MFA para cuentas privilegiadas, limitar intentos de acceso

SBD‑05: verificar propiedad en cada solicitud – el usuario A no puede ver datos del usuario B

SBD‑06: dar a cada componente los permisos mínimos que necesita

Capa 3 – Protección de datos

SBD‑07: ninguna credencial en el código fuente. nunca.

SBD‑08: usar criptografía moderna (AES‑256‑GCM, TLS 1.3). no MD5, no SHA‑1, no Math.random() para tokens.

SBD‑09: recoger solo lo que necesitas. no registrar datos sensibles.

Capa 4 – Resiliencia y monitoreo

SBD‑10: registrar eventos de seguridad con suficiente contexto para investigar un incidente

SBD‑11: fijar límites de tasa en puntos de acceso públicos y topes estrictos en el uso de tokens LLM

SBD‑12: bloquear solicitudes a rangos IP privados (protección SSRF)

SBD‑13: mostrar errores genéricos a los usuarios, registrar detalles en el servidor

Capa 5 – Cadena de suministro y arquitectura

SBD‑14: fijar dependencias, escanear vulnerabilidades, revisar paquetes sugeridos por IA

SBD‑15: fijar acciones de CI/CD a SHAs de commit, no a etiquetas de versión

SBD‑16: verificar sumas de comprobación de modelos antes de cargar LLMs de terceros

SBD‑17: probar prompts del sistema contra intentos de extracción

SBD‑18: filtrar resultados de bases vectoriales por usuario – el usuario A no puede recuperar documentos del usuario B

SBD‑19: validar salidas LLM antes de pasarlas a una base de datos, un shell o un navegador

SBD‑20: establecer CORS en orígenes explícitos, nunca * en puntos de acceso autenticados

SBD‑21: fallar de manera segura – denegar acceso si falla la verificación de permisos

SBD‑22: hacer de la revisión de seguridad parte de tu Definition of Done

SBD‑23: mantener un inventario de cada servicio, API e integración

SBD‑24: definir umbrales de alerta antes de que ocurra un incidente

SBD‑25: identificar regulaciones de privacidad aplicables al inicio del proyecto

Cada control tiene:

una descripción corta del problema

un ejemplo de código que falla la comprobación

un ejemplo de código que pasa

los estándares a los que se refiere (OWASP, NIST, ISO, CIS)

El skill también incluye:

una verificación de versión: la IA te dice si existe una versión más nueva

detección de idioma: responde en inglés, francés o español según tus mensajes

tres niveles: BAJO (proyectos personales), ESTÁNDAR (aplicaciones de negocio), REGULADO (finanzas, salud, gobierno)

reglas anti‑alucinación: la IA solo afirma que algo es seguro si puede mostrarte código que funcione para tu pila

resolución de conflictos: reglas para cuando dos controles tiran en direcciones opuestas (ej. minimización de datos vs registro de seguridad)

Qué no es este skill
SecureByDesign no es una varita mágica.

No:

escanea tu aplicación en ejecución – revisa código y arquitectura, pero no ataca tu sistema

hace cumplir nada en producción – te dice qué hacer; tú tienes que implementarlo

reemplaza una prueba de penetración – para sectores regulados, sigues necesitando una auditoría formal

arregla tu código automáticamente – señala problemas y muestra correcciones, pero tú (o tu IA) escriben el código corregido

verifica todas las vulnerabilidades posibles – cubre 25 patrones comunes, no todos los CWE

El skill es honesto sobre sus límites. Cada informe termina con:

Este análisis cubre patrones de vulnerabilidad conocidos en el código y la arquitectura proporcionados. No reemplaza las pruebas de penetración, el modelado formal de amenazas o una auditoría de seguridad certificada para sistemas que manejan datos sensibles o regulados.

Para quién es
desarrolladores que quieren atrapar errores antes de enviar

equipos que necesitan una base de seguridad coherente entre proyectos

organizaciones sin ánimo de lucro y pequeñas con pocos recursos de seguridad

cualquiera que use una IA para escribir código y quiera evitar los errores más comunes

No necesitas ser un experto en seguridad. El skill hace las comprobaciones.

Prácticas básicas de seguridad que el skill señala
Cuando vas rápido, es fácil olvidar lo básico. El skill lo atrapa.

Lo que el usuario escribe	Lo que el skill señala
password = "admin123" (incluso si está hasheada)	contraseña débil – requiere longitud mínima, complejidad y comprobación contra diccionarios comunes
api_key = "sk-abc123..." en el código	secreto hardcodeado – debe moverse a variables de entorno
SELECT * FROM users WHERE id = + user_id	riesgo de inyección SQL – debe usar consultas parametrizadas
entrada de usuario colocada directamente en el prompt del sistema	riesgo de inyección de prompt – requiere mensajes separados por rol
JWT sin exp o con alg: none	autenticación rota – debe establecer expiración y verificar algoritmo
Access-Control-Allow-Origin: * en un punto de acceso autenticado	mala configuración CORS – requiere lista explícita de orígenes
traza de pila mostrada al usuario	divulgación de información – solo mensajes de error genéricos
sin límite de tasa en /login	riesgo de toma de control de cuenta – límites por IP y por cuenta exigidos
El skill no solo señala. Explica por qué importa y muestra la corrección.

Cómo usarlo
1. Descargar el skill
Ve al repositorio de GitHub y guarda SKILL.md en tu ordenador.
https://github.com/securebydesign/skill/blob/main/SKILL.md

2. Añadirlo a tu IA
Cada herramienta tiene un lugar diferente para las instrucciones. El sitio web tiene guías detalladas para cada una.

Claude.ai – crear un Proyecto, pegar en las instrucciones del Proyecto
ChatGPT – crear un GPT personalizado, pegar en Instrucciones
Claude Code – crear CLAUDE.md en la raíz del proyecto, pegar el skill
Cursor – crear una regla en .cursor/rules/, pegar el skill
Windsurf – Personalizaciones → Reglas → Regla de espacio de trabajo, pegar el skill
Cline – crear .clinerules/securebydesign.md, pegar el skill
Ollama – crear un Modelfile, añadir el skill en la sección SYSTEM
Cualquier API – cargar SKILL.md al inicio, pasarlo como primer mensaje de sistema

Para guías paso a paso: securebydesign.saccessa.com

3. Seguir trabajando
Desde ese momento, cada respuesta que dé tu IA será verificada contra los 25 controles.

Qué ves cuando encuentra algo
Cuando la IA detecta un problema, te lo dice directamente:

text
❌ SBD-07: Secreto en código fuente
   → Clave API en línea 12 de config.py
   → Riesgo: cualquiera con acceso al repositorio puede usar esta clave
   → Corrección: mover a variable de entorno
   → Código:
        import os
        api_key = os.environ["SERVICE_KEY"]   # correcto
        # nunca: api_key = "sk-abc123..."
Si la IA no está segura porque tu pila técnica está mal representada en sus datos de entrenamiento, lo dice:

Mi conocimiento de [X] es limitado. Lo siguiente se basa en principios generales de seguridad. Verifica los detalles en la documentación de [X].

Repositorio
Este repositorio contiene tres archivos:

text
securebydesign/skill/
├── SKILL.md        # el skill en sí – cárgalo en tu IA
├── README.md       # este archivo
└── LICENSE         # licencia MIT – usa libremente, modifica, comparte
Licencia y apoyo
El skill tiene licencia MIT. Úsalo en cualquier proyecto. Modifícalo. Compártelo.

Si quieres apoyar el trabajo:
➡️ Estrella en GitHub – ayuda a otros a encontrarlo
➡️ Contribución económica – financia investigación y mantenimiento

Quienes apoyan con 500 $ o más aparecen en el registro de cambios y en el sitio web. Sin obligación. La licencia MIT es incondicional.

Más información
Sitio web con guías de instalación y explicaciones de los controles:
securebydesign.saccessa.com

Repositorio en GitHub:
github.com/securebydesign/skill

