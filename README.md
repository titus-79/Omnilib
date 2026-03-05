# Omnilib

## Diagramme de Cas d'Utilisation

## Diagramme de Classes

```mermaid
classDiagram

    Livre <|-- EBook
    Livre <|-- VOD
    User <|-- UserStandart
    UserStandart <|-- User Prenium
    User <|-- Bibliothecaire

    Catalogue o-- Livre : contient

    UserStandart --> Livre
    UserStandart --> Catalogue
    UserPrenium --> EBook
    UserPrenium --> VOD
    Bibliothecaire --> UserStandart
    Bibliothecaire --> Catalogue

 
    class Livre {
        - String titre
        - String auteur
        - Date anneeEdition
        - String codeBarre
        - String rayon
        - String etat
    }

    class EBook {
        - int mo
        - String format
    }

    class User {
        <<abstract>>
        - int id
        - String firstName
        - String lastName
    }

    class UserStandart {
        emprunter()
        reserver()
    }

    class UserPrenium {
        telechargerEBook()
        locationVOD()
    }

    class Bibliothecaire {
        majCatalogue()
        validerRetour()
    }

    class Catalogue {
        - int id
        - String name
        naviguer()
        recherche()
        disponibilite()
    }

    class VOD {
        - int duree
        - int resolution
    }

```

## Diagramme d'Activité

### Diagramme globale

```mermaid
flowchart TB
    Start((Debut)) --> CN{Connexion ?}
    CN --OUI--> Auth[Entrez identifiant et mot de passe]
    Auth --> CC{Identification ?}
    CC --Bibliothecaire--> Menu{Menu Visiteur}
    CC --Membre--> Menu
    CC --Prenium--> Menu
    CC --Prenium--> MP{option Prenium}
    CC --Bibliothecaire--> MA{Menu Administrateur}
    CC --erreur-->  CN
    CC --Quitter--> Logout[Deconnexion]
    Logout ---> End((Fin))
    CN --NON--> Menu
    MA --MAJ--> MAJ[Maj catalogue] --> MA
    MA --RETURN--> R[Valider Retour Emprunt] --> MA
    Menu --NAV--> Nav[Navigation]
    Menu --SEARCH--> Search[Recherche]
    MP --DL--> DL[Telechargement]
    MP --VOD--> VOD[VOD]
    Search --> DF{Document trouvé ?}
    Nav --> DF{Document trouvé ?}
    DL --> DF
    VOD --> DF
    DF --OUI & Utilisateur Connecté--> CE{Choix Emprunt} 
    DF --NON & Utilisateur connecté--> CC
    DF --NON & Utilisateur non connecté--> CN
    CE --NON--> CC
    CE --IRL--> EP[Emprunt Physique]
    CE --ONLINE--> EL[Reservation en ligne]
    CE --Download--> EDL[Telecharger]
    EP --> PE{Penalité de retard en cours ?}
    EL --> PE
    EDL -->PE
    PE --OUI--> B[Action Bloquée] --> CC
    PE --Non & DOWNLOAD--> KD[Visionnage Kandle] --> CC
    PE --NON & IRL--> DE[Debut emprunt] --> CC
    
    PE --Non & ONLINE--> DEL{Option Livraison}
    DEL --OUI--> DELOK[livraison Validé] --> DE
    DEL --NON--> DE
```
### Diagramme Algorithme du processus de "Réservation en ligne d'un livre physique"

```mermaid
flowchart TB
Start((Debut)) --> CN{Connexion ?}
    CN --OUI--> Auth[Entrez identifiant et mot de passe]
    Auth --> CC{Identification ?}
    CC --Membre--> Menu
    CC --erreur-->  CN
    CC --Quitter--> Logout[Deconnexion]
    Logout ---> End((Fin))
    Menu --NAV--> Nav[Navigation]
    Menu --SEARCH--> Search[Recherche]
    Search --> DF{Document trouvé ?}
    Nav --> DF{Document trouvé ?}
    DF --OUI & Utilisateur Connecté--> CE{Choix Emprunt} 
    DF --NON & Utilisateur connecté--> CC
    DF --NON & Utilisateur non connecté--> CN
    CE --IRL--> EP[Emprunt Physique]
    CE --NON--> CC
    EP --> PE{Penalité de retard en cours ?}
    PE --OUI--> B[Action Bloquée] --> CC
    PE --NON & IRL--> DE[Debut emprunt] --> CC
    PE --Non & ONLINE--> DEL{Option Livraison}
    DEL --OUI--> DELOK[livraison Validé] --> DE
    DEL --NON--> DE
```