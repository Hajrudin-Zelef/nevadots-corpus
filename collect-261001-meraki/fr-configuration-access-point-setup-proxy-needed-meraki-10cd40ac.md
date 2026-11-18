Meraki
À partir de la version du micrologiciel MR 30.X, les points d’accès Meraki prennent en charge RadSec. Par conséquent, nous recommandons de mettre à jour votre version du micrologiciel et de suivre le guide RadSec pour Meraki à la place.
configuration Meraki
- Dans votre Meraki Tableau de bord allez à Sans fil > SSID
- Activez un nouveau SSID et donnez-lui le nom souhaité 
  - Enregistrez vos modifications
- Modifiez les paramètres de votre SSID 
  - Sous Accès réseau sélectionnez Entreprise avec mon serveur RADIUS
- Ensuite, allez à Serveurs RADIUS et ajoutez vos serveurs RADIUS. Utilisez l’ adresse IP de votre Proxy, le Port 1812 et le Secret partagé depuis votre Paramètres du serveur page
- Configurer les paramètres EAP et les délais d’expiration conformément au ceci guide de référence en allant à Sans fil > Radius > Paramètres RADIUS avancés. Une fois configuré, cela devrait ressembler à la capture d’écran ci-dessous.
- à test que la configuration fonctionne, vous pouvez ajouter un utilisateur dans votre Portail et utiliser la fonction de test Meraki
- Enregistrer vos modifications.
Mis à jour
Ce contenu vous a-t-il été utile ?