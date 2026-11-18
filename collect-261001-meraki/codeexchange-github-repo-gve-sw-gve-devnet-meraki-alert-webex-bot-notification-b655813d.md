Meraki has a broad portfolio including routers, switches, APs, cameras and sensors. The Meraki cloud is the backbone of the highly available, secure, and efficient Meraki solution. It allows for easy management and provisioning of the devices. Moreover, all the products and solutions are programmable, which allows us to build customization and integrations. In this Proof of Value (PoV), we have created an integration between the Meraki sensors, cameras and Webex. Once a Meraki MT sensor is triggered, an alert is sent to a Webex space along with a link to the video wall, which allows all the members of the Webex space to view the cameras an remotely observe the site after a sensor is triggered.
Remote observance is especially useful for offices, for example, and remote observance is in demand due to the fact that we are transitioning towards a hybrid work experience. Moreover, in our use case, there is a bee hive located at the office. For safety reasons, bee hive inspections have to be done with two or more people. In the unlikely event of an accident, the second person can provide assistance or call the emergency services. However, since fewer people are attending the office, it is sometimes inconvenient to have two people doing the inspection. Therefore, we have created a process and integation that allows us to conduct remote observance, where one team member can do the bee hive inspection alone, while team members observe the site remotely and can contact assistance if needed.
In addition to remote observance, the team would also like to monitor the honey bee colony. For this PoV, we have installed three Meraki MV cameras and a BEEP base, which encompasses multiple sensors to monitor a bee hive colony. We have created a bot that would interact during the remote observance process, but we have added features that allows the users to generate snapshots from the Meraki cameras or query the latest metric, e.g. temperature, humidity, weight, etc.
In summary, the bot has the following functionalities:
An overview of the remote observance flow that will take place through the bot:
The technical design of the PoV:
In order to use the Cisco Meraki API, you have to enable the API for your organization first. After having enabled API access, you can generate an API key. You can follow the following instructions on how to enable API access and how to generate an API key:
Log in to the Cisco Meraki dashboard
In the left-hand menu, go to Organization > Settings > Dasbhoard API access
Click on Enable access to the Cisco Meraki Dashboard API
Go to Profile > API access
Under API access, click on Generate API key
Save the API key in a safe place. Please note that the API key will be shown only once for security purposes. In case you lose the key, then you have to revoke the key and regenerate a new key. Moreover, there is a limit of only two API keys per profile.
For more information on how to generate an API key, please click here here
Note: Make sure this API key has write access to both the source and target organization. You can add your account as Full Organization Admin to both organizations by following the instructions here.
In order to get the details from certain devices, we have to specify the serial number. We can retrieve the serial number from the Meraki Dashboard. Follow the following steps in order to retrieve serial number for each device:
Log in to the Cisco Meraki Dashboard.
In the left-hand menu, go to Cameras > Cameras for cameras or Wireless > Access points for access points
Click on one of the device names, e.g. example_camera_1
(optional for cameras) Click on Network
On the left-hand table, you will see the Serial Number. Copy the serial number, because you will need this for the environment variables.
For this PoV, we create a small database with known clients. The list of clients can be retrieved from the Cisco Meraki Dashboard. Follow the following instructions in order to obtain the client details:
In the left-hand menu, go to Network-wide > Clients
Click on one of the devices under description that you would like to add your 'known clients database', e.g. Simons-iPhone
(optional) Change the name to a simple custom name if needed by clicking on it
Copy the name and store it in the environment variables list later
In order to send notifications to a Webex space, we have created a Webex Bot. Follow the following instructions to create a Webex bot and its token:
Log in to developer.webex.com
Click on your avatar and select My Webex Apps
Click Create a New App
Click Create a Bot to start the wizard
Following the instructions of the wizard: fill in details such as the bot name, bot username and choose an icon
Click Add Bot and you will be given access token
Copy the access token and store it safely. Please note that the API key will be shown only once for security purposes. In case you lose the key, then you have to revoke the key and regenerate a new key
For more information about Webex Bots and how to create one, please see the documentation.
You have to specify the Room ID of the Webex space and you have to add the Webex Bot to the room space as well.
In order to obtain the Room ID, you have to make a GET request to the following endpoint:
The response will be a list of JSON objects, which are the spaces that the user is part of. Find the space that you would like to send the notifications to and copy the Room ID.
For more information about how to obtain a Webex Room ID, please consult the following resource here.
BEEP base is developed for bee keepers to collect real-time data about their bee colonies. Metrics that are collected include the temperature, humidity, weight and frequency that is emitted. These metrics allow the bee keepers to assess the health of the bee colonies and help them with the bee keeping overall.
For more information, please refer to their website here
In order to obtain the data that is collected by the BEEP base, we have to pull the data from the BEEP dashboard. In order to make API calls to the dashboard, we have to obtain a token. Please follow the instructions to obtain a token:
    https://api.beep.nl/api/login
BEEP_API_TOKEN.
If you are testing locally, then you need to have a public ip that tunnels back to your local machine. There are many solutions that can provide this service. In this PoV, we have chosen ngrok. If you have not downloaded it yet, then follow the following instructions:
    $ brew install ngrok/ngrok/ngrok
or download the ZIP file via their website and then unzip ngrok from the terminal:
    $ sudo unzip ~/Downloads/ngrok-stable-darwin-amd64.zip -d /usr/local/bin
    $ ngrok authtoken <token>
For more information about the download instruction of ngrok, then head over to their website
The following commands are executed in the terminal.
Create and activate a virtual environment for the project:
 #WINDOWS:
 $ py -3 -m venv [add_name_of_virtual_environment_here] 
 $ [add_name_of_virtual_environment_here]/Scripts/activate.bat
 #MAC:
 $ python3 -m venv [add_name_of_virtual_environment_here] 
 $ source [add_name_of_virtual_environment_here]/bin/activate
For more information about virtual environments, please click here
Access the created virtual environment folder
 $ cd [add_name_of_virtual_environment_here]
Clone this repository
 $ git clone [add_link_to_repository_here]
Access the folder GVE_DevNet_Meraki_Alert_Webex_Bot_Notification
 $ cd GVE_DevNet_Meraki_Alert_Webex_Bot_Notification
Install the dependencies:
 $ pip install -r requirements.txt
Open the .env file and add the environment variables. In the sections above, it is explained how to obtain these credentials and variables. Please note that all the variables below are strings, except for MERAKI_DEVICE_CLIENTS, which is a list of strings.
TEAMS_BOT_EMAIL = "<insert_teams_bot_email>"
TEAMS_BOT_TOKEN = "<insert_teams_bot_token>"
TEAMS_BOT_URL = "<insert_teams_bot_url>"
TEAMS_BOT_APP_NAME = "<insert_teams_bot_app_name>"
WEBEX_ROOM_ID = "<insert_webex_room_id>"
BEEHIVE_ID = "<insert_beehive_id>" # optional
BEEP_API_TOKEN = "<insert_beep_api_token>"
MERAKI_API_KEY = "<insert_meraki_api_key>" MERAKI_NETWORK_ID = "<insert_meraki_network_id>"
MERAKI_CAMERA_SERIAL_FRONT = "<insert_meraki_camera_serial_front>" MERAKI_CAMERA_SERIAL_SIDE = "<insert_meraki_camera_serial_side>" MERAKI_CAMERA_SERIAL_DISTANCE = "<insert_meraki_camera_serial_distance>" MERAKI_CAMERA_SERIAL_AP = "<insert_meraki_camera_serial_ap>" MERAKI_DEVICE_CLIENTS = ["<insert_list_of_names>"]
Now is the time to launch the application! If you are testing locally, then you need to conduct one more step in between using ngrok.
Start an ngrok tunnel using the following command:
    $ ngrok http https://0.0.0.0:5001
You will obtain two public ip addresses. Copy the https address and paste that into the TEAMS_BOT_URL variable in the .env file.
If you have the application running on a server exposed with a public IP address, then copy the https address and paste that into the TEAMS_BOT_URL variable in the .env file.
Now simply type in the following command in your terminal:
$ python bot.py
And head over to the following url:
https://0.0.0.0:5001
Provided under Cisco Sample Code License, for details see LICENSE
Our code of conduct is available here
See our contributing guidelines here
Please note: This script is meant for demo purposes only. All tools/ scripts in this repo are released for use "AS IS" without any warranties of any kind, including, but not limited to their installation, use, or performance. Any use of these scripts and tools is at your own risk. There is no guarantee that they have been through thorough testing in a comparable environment and we are not responsible for any damage or data loss incurred with their use.
You are responsible for reviewing and testing any scripts you run thoroughly before use in any non-testing environment.
Provide links to related white papers:
Provide a link to a related DevNet Sandbox:
Provide links to related Learning Labs or modules on DevNet:
Provide links to related solutions on DevNet Ecosystem Exchange:
Owner
Contributors
Categories
CollaborationIoTNetworkingSecurity
Products
MerakiWebex
Programming Languages
Python
License
Code Exchange Community
Get help, share code, and collaborate with other developers in the Code Exchange community.View Community
Lorsque vous consultez un site Web, celui-ci peut enregistrer ou récupérer des informations présentes dans votre navigateur, le plus souvent sous la forme de témoins. Ces informations peuvent vous concerner, ainsi que vos préférences ou votre périphérique, et servent principalement à faire fonctionner le site selon vos attentes. Généralement, ces informations ne permettent pas de vous identifier directement, mais elles sont utilisées afin de vous offrir une expérience Web personnalisée. Parce que nous respectons votre droit à la vie privée, vous pouvez choisir de ne pas autoriser certains types de témoins. Cliquez sur les différents en-têtes de catégorie pour en apprendre davantage et modifier nos paramètres par défaut. Toutefois, bloquer certains types de témoins peut avoir une incidence sur votre expérience du site et les services que nous proposons.
Ces cookies sont indispensables au bon fonctionnement du site web et ne peuvent pas être désactivés au niveau de nos systèmes. Ils ne résultent généralement que d'actions que vous avez effectuées et qui correspondent à une demande de service, comme lorsque vous définissez vos préférences en matière de confidentialité, que vous vous connectez ou que vous remplissez des formulaires. Vous pouvez configurer votre navigateur pour qu'il bloque ces cookies ou vous avertisse de leur présence, mais certaines parties du site ne seront alors plus opérationnelles. Ces cookies n'enregistrent aucune information d'identification personnelle.
Ces cookies fournissent des indicateurs relatifs aux performances et à la facilité d'utilisation de notre site. Ils visent principalement à récolter des informations sur la façon dont vous interagissez sur le site, notamment le temps de chargement des pages, les temps de réponse, les messages d'erreur et le déroulement des interactions pour chaque visiteur. Nous pouvons ainsi examiner et analyser le comportement des visiteurs pour améliorer l'ergonomie et les fonctionnalités de notre site. Ces cookies nous permettent également de comptabiliser les visites et les sources de trafic afin de mesurer et d'améliorer les performances du site. Ils nous aident à savoir quelles pages sont les plus et les moins populaires, et à voir la manière dont les visiteurs naviguent sur le site. Si vous n'autorisez pas ces cookies, nous ne saurons pas si vous avez déjà visité le site et ne pourrons pas en mesurer les performances.
Ces cookies peuvent être intégrés à notre site par nos partenaires publicitaires. Ils peuvent être utilisés par ces entreprises pour établir un profil de vos centres d'intérêt et vous présenter des publicités pertinentes sur d'autres sites. Si ces cookies n'enregistrent pas directement vos informations personnelles, ils identifient de manière unique votre navigateur et votre terminal d'accès à Internet. Si vous n'autorisez pas ces cookies, des publicités moins ciblées s'afficheront.
Ces cookies permettent d’améliorer et de personnaliser les fonctionnalités du site Web. Ils peuvent être activés par nos équipes, ou par des tiers dont les services sont utilisés sur les pages de notre site Web. Si vous n'acceptez pas ces cookies, une partie ou la totalité de ces services risquent de ne pas fonctionner correctement.