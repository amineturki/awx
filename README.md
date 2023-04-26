
Tout d'abord pour récupérer le projet il faut faire un git clone et passer par un proxy comme suit :
git -c "http.proxy=10.228.196.2:3128" clone https://WissemBenHouria:fsHqqSaN-2DQL4woMeQt@gitlab.tech.orange/stagepfe-sgidf/testflux.git
Une fois tu as le projet localement :
il existe un fichier file.json sous le dossier json_files qui contient la matrice de flux en json format 
chaque attribut du la matrice de flux contient ces attributs
 {
  "Id": ,
  "Source  ip address Mask": "",
  "Hostname": "",
  "Destination ip address Mask": "",
  "Description": "",
  "Proxy": "",         ce champ est facultatif
  "Proxy_Port": ":",   ce champ est facultatif
  "Protocole": "",
  "Service":"",
  "Port": "",
  "Description des flux": ""
 }
 une fois la matrice remplie , il faut générer un inventaire qui contient 
 une liste des machine hotes et leurs variables 
 pour cela il faut executer cette command ( vérifier que vous ete ~/testflux-v2/inventory )
   ansible-playbook python_inventory.yaml
 en effet ce playbook va executer un script python render_yaml.py qui va formater le contenu du json en inventaire sous le yaml format  
 comme résulat un fichier host.yaml sera allimenté
 
L'étape suivante est d'éxucuter le main playbook qui contient des roles 
    - role: prerequis qui installe les outils de test comme curl , telnet si neccesaires dans les machines hotes 
    - role: telnet-test qui lance les differents test vers les destinations
    - role: reports qui genere un rapport détaillé sur l'état de test
    - role: fetch qui envoie les rapports generés vers la machine du ansible controller 
ansible-playbook launch-playbook.yaml -i  inventory/host.yaml --vault-password-file=ansible-vault/vault_pass.txt -v
	comme resulat vous trouverez les rapports portant le nom des machines hotes ainsi que la date exacte d'execution du playbook
    dans le dossier reports	
