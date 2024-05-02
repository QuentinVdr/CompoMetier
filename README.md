# CompoMetier

Repo pour les schéma de cours d'atelier métier

## Diagram de séquence

```mermaid
sequenceDiagram
    actor C as Candidat
    actor CR as Charger de recrutement
    participant Front as Application
    participant Back as Serveur

    loop Pour chaque étape du Process de recrutement
        CR->>+C: Pris de contact
        CR->>+Front: Création du block lié à l'étape de recrutement
        Front->>+Back: Création du block lié à l'étape de recrutement
        Back-->>-Front: Confirmation de l'enregistrement
        Front-->>-CR: Confirmation de l'enregistrement
        C-->>-CR: Réponse
        CR->>+Front: Enregistre la réponse
        Front->>+Back: Enregistre la réponse
        Back-->>-Front: Confirmation de l'enregistrement
        Front-->>-CR: Confirmation de l'enregistrement
    end

    loop Création du dossier candidat
        CR->>+Front: Création du dossier candidat
        Front->>+Back: Création du dossier candidat
        Back->>Back: Création du dossier candidat a partir du template HTML
        Back-->>-Front: Envoi du dossier candidat
        Front-->>-CR: Envoi du dossier candidat
    end

    critical Création du contrat
        CR->>+Front: Création du contrat pour embauché le candidat
        Front->>+Back: Vérification des informations du contrat
    Option S'il manque des données
        Back-->>-Front: Récupérer les informations du candidat manquante
        Front->>+CR: Demande de compléter les informations
        CR->>+C : Demande les informations manquantes
        C->>-CR : Renvoie les informations manquantes
        CR->>+Front: Envoi des informations manquantes
        Front->>+Back: Enregistrement des informations manquantes
        Back-->>-Front: Confirmation de l'enregistrement
        Front-->>-CR: Confirmation de l'enregistrement
        CR->>+Front: Recréation le contrat pour embauché le candidat
    Option Aucune information manquante
        Back->>Back: Création du contrat
        Back-->>Front: Envoi du contrat
        Front-->>CR: Envoi du contrat
        CR->>+C: Envoi du contrat
        C-->>-CR: Réponse
        CR->>+Front: Enregistre la réponse
        Front->>+Back: Enregistre la réponse
        Back-->>-Front: Confirmation de l'enregistrement
        Front-->>CR: Confirmation de l'enregistrement
    end
```

## Diagram d'activité

Diagram d'activité pour la génération de dossier candidat

```mermaid
flowchart TD
    A[Start] --> B{Si les informations du candidat et\nles documents administratif du candidat existent}
    B -- No ----> C[End]
    B -- Yes --> D[Clique sur le bouton pour générer le dossier candidat]
    D --> E{Si toutes les informations sont correctes}
    E -- No --> F[Corriger les informations]
    F --> D
    E -- Yes --> G[Récupère du dossier candidat]
    G --> C
```

## Diagram de cas d'utilisation

```plantuml
@startuml
left to right direction
actor "Chargé de recrutement" as cr
actor "Directeur d'agence" as ad
package ATS {
  usecase "Gérer un candidat" as UC1
  usecase "Créer un candidat" as UC11
  usecase "Modifier un candidat" as UC12
  usecase "Supprimer un candidat" as UC13
  usecase "Gérer un bloc" as UC2
  usecase "Gérer un bloc échange" as UC21
  usecase "Gérer un bloc pre-selection" as UC22
  usecase "Gérer un bloc entretien" as UC23
  usecase "Gérer un bloc information contract" as UC24
  usecase "Gérer un bloc document administratif" as UC25
  usecase "Faire une recherche de candidat" as UC3
  usecase "Consulter les rapports d'agence" as ADUC1
}
cr --> UC1
UC1 --> UC11
UC1 --> UC12
UC1 --> UC13
cr --> UC2
UC2 --> UC21
UC2 --> UC22
UC2 --> UC23
UC2 --> UC24
UC2 --> UC25
cr --> UC3
ad --> ADUC1
@enduml
```

## Merise

```mermaid
classDiagram
    Candidat <|-- SelectionProcess
    Exchange --|> SelectionProcess
    PreSelection --|> SelectionProcess
    Interview --|> SelectionProcess
    HiringProposal --|> SelectionProcess
    PreContract --|> SelectionProcess
```
