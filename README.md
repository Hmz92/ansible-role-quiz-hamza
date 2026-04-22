# ansible-role-quiz-hamza

Rôle Ansible **complet et portable** déployant l'application Node.js
`quiz-ansible` depuis son dépôt GitLab sur tout système Linux conteneurisé
de type **Debian / Ubuntu** ou **RedHat / Rocky Linux**.

## Fonctionnalités

- Détection automatique de la famille d'OS (`ansible_os_family`)
- Installation de Node.js via le dépôt **NodeSource**
  - `apt` + `deb.nodesource.com` sur Debian / Ubuntu
  - `dnf` + `rpm.nodesource.com` sur RedHat / Rocky
- Clone du dépôt GitLab `quiz-ansible`
- Installation des dépendances `npm`
- Build de l'application (`npm run build`)
- Installation globale de `serve`
- Lancement de l'application sur le port configuré

## Prérequis

- Ansible `>= 2.10`
- Collection `community.general` (pour le module `npm`)
- Accès `root` ou `sudo` sur les hôtes cibles

```bash
ansible-galaxy collection install community.general
```

## Variables par défaut (`defaults/main.yaml`)

| Variable        | Valeur par défaut                                       | Description                       |
|-----------------|---------------------------------------------------------|-----------------------------------|
| `app_repo`      | `https://gitlab.com/ftutorials-labs/quiz-ansible.git`   | URL du dépôt GitLab               |
| `app_branch`    | `main`                                                  | Branche à cloner                  |
| `app_dir`       | `/opt/quiz-ansible`                                     | Emplacement du code sur l'hôte    |
| `app_port`      | `3000`                                                  | Port d'écoute de `serve`          |
| `node_version`  | `23.x`                                                  | Version Node.js (NodeSource)      |
| `app_user`      | `root`                                                  | Propriétaire des fichiers         |
| `app_group`     | `root`                                                  | Groupe propriétaire               |

## Utilisation

### Playbook minimal

```yaml
---
- name: Déployer quiz-ansible via le rôle
  hosts: all
  become: yes
  roles:
    - ansible-role-quiz-hamza
```

### Avec variables personnalisées

```yaml
---
- hosts: all
  become: yes
  roles:
    - role: ansible-role-quiz-hamza
      vars:
        app_port: 8080
        node_version: "22.x"
        app_dir: /srv/quiz
```

### Exécution

```bash
ansible-playbook -i inventaire playbooks/ansible-role-quiz-hamza.yaml
```

## Tags disponibles

| Tag        | Actions                                   |
|------------|-------------------------------------------|
| `packages` | Mise à jour + paquets système             |
| `nodejs`   | Installation Node.js (NodeSource)         |
| `app`      | Clone, build et installation de `serve`   |
| `service`  | Lancement de l'application                |

Exemple : `ansible-playbook ... --tags "nodejs,app"`

## Publication sur Ansible Galaxy

```bash
# 1. Initialiser un dépôt git local pour le rôle
cd roles/ansible-role-quiz-hamza
git init && git add . && git commit -m "Initial role"

# 2. Pousser sur GitHub (obligatoire pour Galaxy)
git remote add origin git@github.com:<user>/ansible-role-quiz-hamza.git
git push -u origin main

# 3. Récupérer le token depuis https://galaxy.ansible.com/me/preferences
ansible-galaxy role import --api-key <TOKEN> <user> ansible-role-quiz-hamza
```

## Auteur

Hamza Assas — Exercice 32 (Déploiement d'une application web via rôle).

## Licence

MIT
