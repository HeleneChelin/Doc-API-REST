# DOCUMENTATION API REST TANAGURU V0.1

Quickstart Api Tanaguru

## RESUME

Use Api Tanaguru with a chrome extension

## Team Tanaguru

www.tanaguru.com

## INTRODUCTION 

L’API REST fournit une API de services Web pratique, puissante, simple pour interagir avec le service d’audit des pages web de Tanaguru. Elle présente comme avantages une grande facilité d'intégration et de développement. C’est également une excellente solution pour une utilisation avec des applications mobiles et Web. 

L’utilisation de l'API exige une connaissance de base des services Web. 

URL de base de l'API : https://www.api.tanaguru.com/v1.0/service

Tous les appels de l'API commencent par cette URL à laquelle s'ajoute le chemin vers l'action désirée. 

Exemple : https://www.api.tanaguru.com/v1.0/service/auditPage 

L'API est accessible uniquement via le protocole Https et accepte les requêtes HTTP POST et GET. Les données envoyées en POST doivent respecter le format JSON.

## AUTHENTIFICATION 

L’API REST utilise le protocole OAuth pour permettre aux utilisateurs d'applications d'accéder en toute sécurité aux données sans avoir à révéler nom d'utilisateur et mot de passe.

Avant de procéder à des appels d'API REST, vous devez vous authentifier en utilisant OAuth 2.0. Pour ce faire, vous aurez besoin des éléments suivants : 

* Un ID Client et le code secret fournis par Océane consulting. 
* Faire une requête POST security/authenticate pour échanger ces informations d’identification et générer un jeton d’authentification via l’API. Ensuite pour toutes les requêtes, vous devez préciser dans le paramètre d’authentification sur le header de la requête, le jeton comme valeur.

![Shema d'authentification](https://raw.githubusercontent.com/Tanaguru/Doc-API-REST/master/assets/capture06.png)

**Les jetons sont des mots de passe**

Gardez à l'esprit que le ID client, le code secret et le jeton sont indispensables pour accéder aux demandes liées à une application. Ces valeurs doivent être considérées comme sensibles et tout comme les mots de passe ne doivent pas être partagées ou distribuées à des personnes non fiables. 

**SSL absolument nécessaire**

Cette authentification n’est fiable uniquement que si SSL est utilisé. Par conséquent, toutes les demandes pour obtenir et utiliser les jetons doivent utiliser des paramètres HTTPS, c’est également une exigence de l'API.

### ÉTAPE 1 : ENCODER LA CLE ET LE CODE SECRET DU CLIENT

Etapes pour encoder l’ID Client et le code secret du client dans un ensemble d'informations d'identification afin d’obtenir un jeton :

1. Concaténer l’ID Client plus le caractère de deux points " :", et le code secret du client, le tout en une seule chaîne.
2. Encoder en Base64 la chaîne obtenue à partir de l'étape précédente.

Vous trouverez ci-dessous des exemples de valeurs indiquant le résultat de cet algorithme. Notez que le code secret de client utilisé pour ces exemples est désactivé et ne fonctionnera pas pour les demandes réelles. 

| Clé | b9025c56b2425053dc069585390ab7c8 | 
| Code secret | 10ee8cf96f9e35f7207a9a5cb3f89ed63c5f59692e1782d82c7eb73d21067695 | 
| Concaténation | b9025c56b2425053dc069585390ab7c8 : 10ee8cf96f9e35f7207a9a5cb3f89ed63c5f59692e1782d82c7eb73d21067695 | 
| Porteur d’informations d’authentification (Base64) | YjkwMjVjNTZiMjQyNTA1M2RjMDY5NTg1MzkwYWI3Yzg6MTBlZThjZjk2ZjllMzVmNzIwN2E5YTVjYjNmODllZDYzYzVmNTk2OTJlMTc4MmQ4MmM3ZWI3M2QyMTA2NzY5NQ== | 

### ÉTAPE 2 : OBTENIR UN JETON

La valeur calculée à l'étape 1 doit être échangée contre un jeton en émettant une demande de POST /service/security/authenticate :

* La demande doit être une requête HTTP POST.
* La demande doit inclure un en-tête Authorization avec la valeur de base : Basic < valeur codée de l'étape 1>.
* La demande doit inclure un en-tête Content-Type avec la valeur application/json.
* La demande doit inclure un en-tête Grant_type : client_credentials. 

Exemple de demande :

`
POST /v1.0/service/security/authenticate HTTP/1.1
Host: api.tanaguru.com
User-Agent: My App v1
Authorization: Basic YjkwMjVjNTZiMjQyNTA1M2RjMDY5NTg1MzkwYWI3Yzg6MTBlZThjZjk2ZjllMzVmNzIwN2E5YTVjYjNmODllZDYzYzVmNTk2OTJlMTc4MmQ4MmM3ZWI3M2QyMTA2NzY5NQ==
Content-Type: application/json
Grant_type: client_credentials
`

Si la demande a été correctement formatée, le serveur répond avec un payload JSON codé :

Exemple de réponse :

`
HTTP/1.1 200 OK
Status: 200 OK
Content-Type: application/json; charset=utf-8
...
{‘’status’’: 200 ,’’token_type’’: ‘’bearer’’ , ‘’access_token’’: ‘’AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA%2FAAAAAAAAAAAA’’}
`

Les demandes doivent vérifier que la valeur associée à la clé token_type de l'objet retourné est « **bearer** ». La valeur associée à la clé access_token est le jeton.

Notez qu'un jeton est valide pour une application à la fois. Refaire une autre demande avec les mêmes informations d'identification va retourner le même jeton jusqu'à ce qu'il soit invalidé. 

### ÉTAPE 3 : AUTHENTIFIER LES REQUETES API AVEC LE JETON

Le jeton est utilisé pour faire des appels vers l’API. Pour utiliser le jeton, construire une demande HTTPS normale et inclure un en-tête **Authorization** avec la valeur du jeton : **Bearer < valeur de jeton de l'étape 2>**. La signature n’est pas nécessaire.

Exemple de demande : 

`
GET /v1.0/service/limit_stat HTTP/1.1

Host: api.tanaguru.com

User-Agent: My App v1.0

Authorization: Bearer AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA%2FAAAAAAAAAAAAAAA
`

## CRÉDITS

L'API de Tanaguru repose sur un **système de crédits**.

Lorsque vous lancez un audit depuis l'API, et que celle-ci génère un résultat finale complet, votre compte est débité d'une unité de crédit.

Les autres requêtes, (analyse échouée, demande d’information statistique, authentification, etc) ne débitent aucun crédit.

## STATUT DU RETOUR, LISTE DES CODES

Chaque appel à l'API donne lieu à une réponse retournant un code spécifique en fonction du résultat obtenu. L'analyse de ce code vous permet de vous assurer que la requête a été traitée avec succès.

Tous les codes >= 400 indiquent que la requête n'a pas été traitée avec succès par nos serveurs.

* **200**: OK
* **400**: Paramètre manquant, ou valeur incorrecte
* **401**: Authentification nécessaire (jeton non précisé ou invalide)
* **403**: Action non autorisée (crédits épuisés, URL non autorisée, etc)
* **404**: Page inaccessible (URL inconnue / impossible d'accéder à l'adresse)
* **406**: Le JSON indiqué en données POST n'est pas valide
* **408**: Dépassement du temps maximal autorisé pour l’audit
* **500**: Erreur inconnue, contactez-nous
* **503**: L'API est momentanément indisponible, réessayez dans quelques minutes

## LISTE DES ACTIONS DISPONIBLES

### LANCER UN AUDIT DE PAGE 

`**POST** https://www.api.tanaguru.com/v1.0/service/auditPage`

**Format de la requête**
`
**Headers** 
**Authorization:** bearer eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI.oGxndCq3Ocz6CKi2aUb4uA
**Accept:** application/json; charset=utf-8
**Content-Type:** application/json

**Payload**
**{**
**"page_url": "",**
**"referentiel ": "",** 
**"level ": "",** 
**"language": "",** 
**"dt_tbl_marker": "" ,**
**"cplx_tbl_marker": "",**
**"pr_tbl_marker": "",** 
**"dcr_img_marker": "",** 
**"inf_img_marker": "",**
**"screen_width ": 1920,** 
**"screen_height": 1080,**
**"description_ref": true,**
**"html_tags": false**
**}**
`

**Paramètres**

