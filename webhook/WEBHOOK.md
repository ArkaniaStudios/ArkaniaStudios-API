# Les WebHooks

Les WebHooks sont des appels HTTP qui sont envoyés à une URL spécifique lorsque certains événements se produisent. Ils sont utilisés pour notifier des applications de l'arrivée de nouveaux messages, de nouveaux utilisateurs, etc.

# Créer un WebHook discord

Pour créer un WebHook discord, il faut aller dans les paramètres du serveur, puis dans la catégorie WebHooks. Il faut ensuite cliquer sur le bouton "Créer un WebHook".

# Envoyer un webhook avec du code

Pour envoyer un webhook avec du code, il vous faudra 4 fichiers : 
- Webhook.php
- Message.php
- Embed.php
- DiscordWebhookTask.php

Chaque fichier aura un but précis. 

### **Webhook.php** : 
Ce fichier est un peu le fichier racine de notre système. Il comportera ces 3 fonctions :

`````php

/** @var string */
protected string $url;

public function __construct(string $url){
    $this->url = $url;
}
`````

Ce construct va nous permet de définir l'url du webhook que nous allons utiliser.

`````php
/**
 * @return string 
 */
public function getURL(): string {
    return $this->url;
}
`````

Cette fonction permettra de récupérer l'url du webhook

`````php
/**
 * @return bool  
 */
public function isValid(): bool {
    return filter_var($this->url, FILTER_VALIDATE_URL) !== false;
}
`````

Cette fonction permettra de vérifier si l'url est valide ou non.

`````php
/**
 * @param Message $message
 * @return void
 */
public function send(Message $message): void{
        Server::getInstance()->getAsyncPool()->submitTask(new DiscordWebhookSendTask($this, $message));
}
`````

Cette fonction permettra d'envoyer le message sur le webhook.

### **Message.php** :

Ce fichier permettra de créer un message qui sera envoyé sur le webhook. Il comportera ces 6 fonctions :

`````php
class Message implements JsonSerializable {

    /** @var array */
    protected array $data = [];

    /**
     * @param string $content
     * @return void
     */
    public function setContent(string $content): void{
        $this->data['content'] = $content;
    }

    /**
     * @return string|null
     */
    public function getContent(): ?string{
        return $this->data['content'];
    }

    /**
     * @return string|null
     */
    public function getUsername(): ?string{
        return $this->data['username'];
    }

    /**
     * @param string $username
     * @return void
     */
    public function setUsername(string $username): void{
        $this->data['username'] = $username;
    }

    /**
     * @param Embed $embed
     * @return void
     */
    public function addEmbed(Embed $embed): void {
        if(!empty(($arr = $embed->asArray()))){
            $this->data['embeds'][] = $arr;
        }
    }

    /**
     * @return array
     */
    public function jsonSerialize(): array {
        return $this->data;
    }

}
``````

La fonction ``setContent()`` permettra de définir le contenu du message.
La fonction ``getContent()`` permettra de récupérer le contenu du message.
La fonction ``setUsername()`` permettra de définir le nom de l'utilisateur qui envoie le message.
La fonction ``getUsername()`` permettra de récupérer le nom de l'utilisateur qui envoie le message.
La fonction ``addEmbed()`` permettra d'ajouter un embed au message.
La fonction ``jsonSerialize()`` permettra de récupérer le message sous forme de tableau.

### **Embed.php** :

Ce fichier permettra de créer un embed qui sera envoyé sur le webhook. Il comportera ces 6 fonctions :

`````php
class Embed {

    /** @var array */
    protected array $data = [];

    public function asArray(): array{
        return $this->data;
    }

    /**
     * @param string $name
     * @param string|null $url
     * @param string|null $iconURL
     * @return void
     */
    public function setAuthor(string $name, string $url = null, string $iconURL = null):void{
        if(!isset($this->data['author'])){
            $this->data['author'] = [];
        }
        $this->data['author']['name'] = $name;
        if($url !== null){
            $this->data['author']['url'] = $url;
        }
        if($iconURL !== null){
            $this->data['author']['icon_url'] = $iconURL;
        }
    }

    /**
     * @param string $title
     * @return void
     */
    public function setTitle(string $title):void{
        $this->data['title'] = $title;
    }

    /**
     * @param string $description
     * @return void
     */
    public function setDescription(string $description):void{
        $this->data['description'] = $description;
    }

    /**
     * @param int $color
     * @return void
     */
    public function setColor(int $color):void{
        $this->data['color'] = $color;
    }

    /**
     * @param string $name
     * @param string $value
     * @param bool $inline
     * @return void
     */
    public function addField(string $name, string $value, bool $inline = false):void{
        if(!isset($this->data['fields'])){
            $this->data['fields'] = [];
        }
        $this->data['fields'][] = [
            'name' => $name,
            'value' => $value,
            'inline' => $inline,
        ];
    }

    /**
     * @param string $url
     * @return void
     */
    public function setThumbnail(string $url):void{
        if(!isset($this->data['thumbnail'])){
            $this->data['thumbnail'] = [];
        }
        $this->data['thumbnail']['url'] = $url;
    }

    /**
     * @param string $url
     * @return void
     */
    public function setImage(string $url):void{
        if(!isset($this->data['image'])){
            $this->data['image'] = [];
        }
        $this->data['image']['url'] = $url;
    }

    /**
     * @param string $text
     * @param string|null $iconURL
     * @return void
     */
    public function setFooter(string $text, string $iconURL = null):void{
        if(!isset($this->data['footer'])){
            $this->data['footer'] = [];
        }
        $this->data['footer']['text'] = $text;
        if($iconURL !== null){
            $this->data['footer']['icon_url'] = $iconURL;
        }
    }

    /**
     * @param DateTime $timestamp
     * @return void
     */
    public function setTimestamp(DateTime $timestamp):void{
        $timestamp->setTimezone(new DateTimeZone('UTC'));
        $this->data['timestamp'] = $timestamp->format('Y-m-d\TH:i:s.v\Z');
    }

}
``````
La fonction ``asArray()`` permettra de récupérer l'embed sous forme de tableau.
La fonction ``setAuthor()`` permettra de définir l'auteur de l'embed.
La fonction ``setTitle()`` permettra de définir le titre de l'embed.
La fonction ``setDescription()`` permettra de définir la description de l'embed.
La fonction ``setColor()`` permettra de définir la couleur de l'embed.
La fonction ``addField()`` permettra d'ajouter un champ à l'embed.
La fonction ``setThumbnail()`` permettra de définir l'image de l'embed.
La fonction ``setImage()`` permettra de définir l'image de l'embed.
La fonction ``setFooter()`` permettra de définir le footer de l'embed.
La fonction ``setTimestamp()`` permettra de définir le timestamp de l'embed.

### **DiscordWebhookTask.php**:

Ce fichier permettra de créer une tâche qui sera exécutée par le serveur.

`````php
class DiscordWebhookSendTask extends AsyncTask {

    /** @var Webhook */
    protected Webhook $webhook;
    /** @var Message */
    protected Message $message;

    public function __construct(Webhook $webhook, Message $message){
        $this->webhook = $webhook;
        $this->message = $message;
    }

    public function onRun() : void{
        $ch = curl_init($this->webhook->getURL());
        curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($this->message));
        curl_setopt($ch, CURLOPT_POST,true);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
        curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, false);
        curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
        curl_setopt($ch, CURLOPT_HTTPHEADER, ["Content-Type: application/json"]);
        $this->setResult(curl_exec($ch));
        curl_close($ch);
    }

    public function onCompletion() : void{
        $response = $this->getResult();
        if($response !== ""){
            Core::getInstance()->getLogger()->error("[DiscordWebhookAPI] Got error: " . $response);
        }
    }

}
``````

⚠ _Les fonctions curl permettent d'effectuer un lien entre une requête internet ainsi que le serveur. Il est important de faire ces requêtes en asynchrone afin de ne pas bloquer le thread principal._