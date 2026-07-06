# Réservation publique avec choix des places + validation troupe

Fichiers à ajouter à la racine du dépôt GitHub :

- `reservation-public.html`
- `demandes-public.html`
- `README_PUBLIC_PLAN.md`

Ne remplace pas `salle.html`.

## Liens après publication

- Page public : `https://orleanne03.github.io/theatre-saint-martin/reservation-public.html`
- Page troupe : `https://orleanne03.github.io/theatre-saint-martin/demandes-public.html`

## Fonctionnement

1. Le public choisit les places sur le plan.
2. À l’envoi, les places sont bloquées temporairement pendant 24 h.
3. Un email récapitulatif est envoyé au spectateur.
4. Un email d’alerte est envoyé à `nellylac@gmail.com`.
5. La demande est visible dans `demandes-public.html`.
6. La troupe peut :
   - valider et réserver définitivement,
   - envoyer automatiquement l’email de confirmation,
   - refuser et libérer,
   - contacter,
   - archiver,
   - exporter en CSV.

## Le trio ajouté

### 1. Email au spectateur après sa demande

Après l’envoi du formulaire public, le spectateur reçoit un email qui confirme que sa demande est bien reçue et que ses places sont bloquées temporairement.

### 2. Bouton “Valider + envoyer confirmation”

Dans `demandes-public.html`, le bouton de validation confirme les sièges dans Firebase puis envoie l’email de confirmation au spectateur.

### 3. Expiration automatique des blocages

Les demandes non validées expirent après 24 h :

- les sièges en attente sont libérés ;
- la demande passe en statut `expiree` ;
- le filtre “Expirées” est disponible dans la page troupe.

Important : comme c’est une application statique, le nettoyage se fait quand la page publique ou la page troupe est ouverte/actualisée. Pour un nettoyage en arrière-plan même quand personne n’ouvre la page, il faudrait un Worker Cloudflare programmé.

## Données Firebase utilisées

Cette version utilise des chemins séparés pour ne pas toucher aux anciennes réservations :

- `spectacles/amies_2026_10_11/seats`
- `spectacles/amies_2026_10_11/holds`
- `spectacles/amies_2026_10_11/public_requests`

## Email

Les emails passent par le Worker Cloudflare/Brevo déjà utilisé par l’application :

`https://brevo-mailer.theatre-sm-03.workers.dev/`

Si une notification email échoue, la demande reste enregistrée dans Firebase. L’envoi de l’email ne doit pas faire perdre une demande.

## Sécurité

Le mot de passe dans `demandes-public.html` reste une protection légère, car il est côté navigateur. Pour une vraie mise à disposition publique durable, il faudra renforcer les règles Firebase ou passer par un Worker/back-end.
