Merci pour ton avis !
Explique-nous pourquoi ce contenu n’est pas utile.
      Comment puis-je faire fonctionner l'IA localement ? Et quel est le modèle / logiciel le plus efficace ? 
        
        
        
    
    
    Salut tout le monde. J'avoue - Sam Altman et Open AI me donnent vraiment une mauvaise impression. Et pour être honnête, même s'ils ont de bonnes intentions et se soucient vraiment du bien-être des gens et font de leur mieux pour garder les conversations privées, quelqu'un pourrait simplement pirater le serveur et fuites les données que les utilisateurs ont. Il sera également contraint de le faire si une loi frivole ou un procès est déposé, de donner des données à des personnes qui n'ont peut-être pas les meilleures intentions ou qui pourraient abuser d'une panique morale comme la sécurité des enfants ou la santé mentale pour des raisons de pouvoir. Ne vous méprenez pas, ces questions doivent être prises au sérieux - mais elles sont souvent utilisées comme un cheval de Troie par les politiciens pour abuser du pouvoir.
Et maintenant qu'ils donnent automatiquement ces données à la police - je suis plus préoccupé. Les départements de police sont pleins de corruption et d'abus de pouvoir, tout comme les tribunaux. Etc.
Mais cette technologie est incroyable. Je pense que lorsqu'elle est utilisée correctement - comme un outil pour aider les gens, permettre aux gens d'apprendre et d'être plus créatifs, cela pourrait vraiment améliorer l'humanité. J'étais curieux. Quel logiciel puis-je utiliser pour émuler ça sur mon propre matériel ? J'ai essayé Ollama, mais j'ai entendu dire que ce n'était pas des plus à jour même si je suis encore bordellement impressionné. Et quel modèle est le meilleur et le plus avancé / le meilleur pour le local ? Je suis un total noob à ce sujet.
Section des commentaires
Le plus convivial pour les débutants et facile à configurer est LM Studio
Il peut également héberger un serveur local sur votre machine pour l'intégrer à d'autres applications d'IA.
Très bien. À votre avis, pourquoi cela pourrait-il être mieux qu'Ollama ?
C'est juste plus facile à configurer et ça vient avec une interface graphique.
Je n'ai utilisé Ollama que pendant une courte période moi-même, mais il ne semblait pas avoir les mêmes performances que LM Studio.
Téléchargez LM Studio. C'est facile à prendre en main et il vous suggérera ce qui fonctionnera sur votre matériel. Pour les modèles, vous pouvez vérifier GPT-OSS 20B ou Qwen3 30B - ce sont des petits modèles efficaces. Si ça dit que vous n'avez pas assez de mémoire pour les gérer, essayez Gemma3 4B.
Essayez-les et voyez si ça vous plaît. Si c'est le cas, vous devriez vous renseigner sur des sujets comme :
- Créer une personnalité et un personnage par les invites système
- Appel d'outils (par exemple, recherche sur le web)
- Dense vs MoE
- Quantification (par exemple, au format GGUF)
- Matériel (inférence GPU vs CPU, Apple Silicon vs PC)
Après avoir appris ça, vous aurez une bien meilleure idée des LLM locaux et vous maximiserez la performance de votre matériel. Cela dit, ce ne sera jamais comparable à la performance de ChatGPT et vous devrez probablement dépenser de l'argent pour faire fonctionner de meilleurs modèles. Si vous voulez vérifier à quel point les réponses seront meilleures avec des modèles plus grands, vous devriez vous inscrire sur OpenRouter et discuter avec des modèles là-bas - par exemple, comparez GPT-OSS 20B avec la variante 120B.
Ensuite, vous pourriez aimer essayer une interface différente, je recommande Open WebUI. Cependant, vous devriez commencer avec LM Studio car il a une interface utilisateur beaucoup plus facile pour apprendre les bases.
Je viens de télécharger LM Studio. C'est incroyable.
Et par interface différente - tu veux dire qu'il y a une option pour faire en sorte que LM Studio ressemble au site web de ChatGPT?
Oui. Open WebUI fait exactement cela et te permet d’ajouter des outils facilement. Le modèle peut ensuite appeler des choses comme :
- Recherche sur le web (Google/Brave/ n'importe quel autre moteur de recherche, grâce à cela, même un petit modèle efficace peut avoir des connaissances à jour d'expert. Tu peux utiliser un VPN pour améliorer la confidentialité de cet outil) - Analyseur de fichiers/OCR (tu peux télécharger un PDF ou un JPG et il verra ce qu'il y a à l'intérieur) - Exécution de code (les LLM ne comptent pas les valeurs avec précision, donc au lieu de compter sur leurs performances, tu les laisses écrire quelques lignes de code puis exécuter. Des résultats beaucoup plus précis.)
De plus, tu as des outils comme la reconnaissance vocale, tu peux facilement changer entre différentes personnalités, etc.
Une personnalité peut être un expert en médecine qui fait beaucoup de raisonnement, essaie de valider ses propres découvertes par une recherche sur le web. Une autre peut être un écrivain créatif, qui se fiche de la factualité ou de la réalité... tu peux obtenir la réponse que tu veux en sculptant le prompt du système. C'est pourquoi tant de gens s'intéressent aux LLM locaux.
Il y a une heure, j'ai aussi réessayé Gemma3 E4B, je détestais ce modèle parce qu'il est beaucoup trop censuré (les mots d'argot populaires sont mal interprétés comme un langage haineux, nuisible ou parfois même suicidaire), mais je vois qu'il y a maintenant une version 'ablitérée'. Cette version ablitérée n’a plus ce problème et la qualité des réponses multilingues est toujours au même niveau, très élevé, donc si tu t’intéresses à certaines langues moins populaires, tu devrais vraiment essayer ce modèle. Gemma3 E4B est différent de Gemma3 4B.
Tu peux lire à propos de l'ablitération, ça t'aidera à comprendre comment fonctionnent les LLM.
https://huggingface.co/blog/mlabonne/abliteration
Je recommande jan.ai au lieu de LM studio parce que jan est open source. Les deux sont meilleurs qu'ollama. Pour le meilleur modèle, cela dépend de ton matériel (en particulier ta VRAM et ta RAM) et de tes besoins.
Tailscale + openwebui + ollama est vraiment une super configuration. J'utilise mon LLM local sur mon téléphone quand je suis en déplacement et je n'ai pas besoin d'être lié à mon bureau.
Comment pouvez-vous utiliser le LLM sur votre téléphone ?
Si tu veux faire du jeu de rôle avec des personnages, tu pourrais aimer mon appli, HammerAI. Elle intègre Ollama et s'installe en un clic.
Tu peux jeter un œil à LM Studio, il y a beaucoup de modèles que tu peux télécharger là-bas.
Concernant le meilleur modèle, cela dépend vraiment de ton matériel. En gros, si tu as un GPU avec 24 Go de VRAM, tu peux généralement faire tourner des modèles 24B. Si tu as 12 Go, tu seras limité à des modèles 12B. LM Studio te dira quels modèles tu peux ou ne peux pas faire fonctionner. Essaie de télécharger des modèles qui indiquent que le déchargement GPU complet est possible.
Aussi, opte pour des modèles avec une quantification plus élevée. Tu peux penser à la quantification comme à une compression, elle réduit l'utilisation de la RAM, mais cela peut aussi affecter les performances (q3 < q4 < q5, et ainsi de suite). Je ne recommanderais pas vraiment d'utiliser des modèles avec une quantification inférieure à q4 ou q3.
Pour de bons modèles, consulte Gemma 3, les modèles Mistral (mistral small est un bon modèle), GPT-OSS, et les modèles Qwen.
Choses à considérer :
llama.cpp - A un fichier exe compilé que vous pouvez télécharger pour l'exécuter. Lance un serveur compatible OpenAIavec une interface web. C'est votre meilleure option si vous voulez quelque chose de léger, qui fonctionne avec la plupart des trucs disponibles, et qui ne vous gêne pas avec une interface utilisateur inutile. Il est capable de décharger le CPU, utilisant à la fois le CPU et le GPU.
Koboldcpp - Pareil que llama.cpp, mais a une interface sympathique pour lancer des choses. Juste au cas où vous ne voulez pas vous fatiguer avec des applications terminal, en apprenant les paramètres de lancement qu'elles ont.
Ollama - est PRESQUE identique à llama.cpp, mais a été dérivé dans son propre petit jardin clos, avec ses propres modèles "requantisés". Ollama était le premier outil à télécharger des modèles pour vous, mais llama.cpp peut déjà le faire tout seul.
LM studio - Je n'en ai aucune idée, car je ne l'ai jamais utilisé. J'ai entendu dire qu'il ne peut pas utiliser votre RAM ou utiliser votre CPU ? Je pourrais le confondre avec VLLM, cependant.
VLLM - Pas d'idée non plus.
L’interface web de génération de texte d’Oobabooga - Était le premier outil "tout inclus". Comparé aux outils ci-dessus, celui-ci peut exécuter une multitude de types de quants différents, comme ExLlamav2, ExLlamav3, Llama.cpp, Transformers, etc. Mais il reste toujours un peu à la traîne niveau mise à jour.
Ensuite, il y a plein d'autres applications, ou façons d'inférer vos modèles, mais je considérerais que ces applications sont divisées en 2 équipes différentes :
Équipe 1, ce sont des applications de fichiers exe C++. Utilisées principalement pour exécuter vos trucs sur votre PC avec peu ou pas de prérequis (excepté peut-être des pilotes GPU et des trucs accélérés par GPU comme CUDA).
Équipe 2, ce sont des applications de "l'enfer Python". Vous avez besoin d'au moins les bases de Python, et des outils Python essentiels pour les faire fonctionner. En raison de la nature de Python en tant que langage de programmation, vous ne passerez peut-être pas un très bon moment à essayer différentes applications Python, car elles peuvent planter à cause des environnements ou des conflits de versions de dépendances.
Quoi que vous choisissiez, gardez à l'esprit que si vous rencontrez un problème étrange, n'hésitez pas à consulter l'onglet "Issues" sur la page Github du projet correspondant. Parfois, des choses "anciennes, éprouvées et vraies" peuvent se retrouver cassées, et vous devrez peut-être essayer autre chose, ou attendre un certain temps avant que les gens compétents aient une chance de le réparer. C'est la nature de cette "toute nouvelle chose".
Oh, et il y a une autre façon de les diviser :
CPU+GPU.
GPU uniquement.
Plusieurs GPU uniquement (outils professionnels pour servir un grand nombre de clients).
La plupart d'entre nous n'utilisent que 1. Mais si vous êtes un passionné avancé avec un extra à dépenser, vous pourriez trouver que 2 et 3 sont plus à votre goût.
Super aperçu. Je peux partager quelques backends supplémentaires à considérer selon le cas d'utilisation :
Pour les grands modèles MoE, ik_llama.cpp est l'un des meilleurs backends. J'ai partagé des détailsicisur la façon de le construire et de l'utiliser au cas où quelqu'un voudrait essayer s'il ne l'a pas déjà fait. Ça peut être beaucoup plus rapide comparé à llama.cpp, surtout pour des tailles de contexte plus grandes. Il est notable qu'ik_llama.cpp vient avec un frontend Mikupad intégré. Mais bien sûr, tout autre frontend qui prend en charge l'API compatible avec OpenAIpeut y être connecté, comme SillyTavern ou Open WebUI.
TabbyAPI (exllamav2/v3) est une excellente option lorsque le modèle tient entièrement dans la VRAM, car cela a tendance à fournir de meilleures performances comparé à d'autres backends.
VLLM, déjà mentionné dans votre aperçu, est l'une des meilleures options pour l'inférence par lots et les configurations multi-utilisateurs sur des machines avec beaucoup de VRAM, car il est moins efficace en mémoire d'après mon expérience, et aussi plus compliqué à configurer.
Détermine quel niveau de modèle tu veux exécuter. MoE fonctionne très bien sur la plupart des configurations RAM+CPU. Dis-nous ta configuration pour plus d'infos.
Je te recommanderais KoboldCPP. Tu n'as qu'à exécuter le fichier exe et charger le modèle, et elle a plein de fonctionnalités qui manquent à d'autres plateformes comme LMStudio, comme par exemple le "mode histoire/autocomplétion pure" et les cartes de personnages.
Je trouve aussi que l'API est beaucoup plus compatible.
Que veux-tu en faire ? Quel matériel as-tu ?