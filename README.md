# Utilizando o KUBECOST para monitorar nós Spots da AWS
Monitorando valores dos nós Spots com KUBECOST

# 1. Instalando o KUBECOST

Faça a instalação do Kubecost conforme o link:

https://kubecost.com/install?ref=home

# 2. Adicionando labels em seu nó Spot

Para que a monitoração dos custos de nós SPOTs funcionem, é necessário identificar quais nós são spots através de Labels, para adicionar labels em seu nó, siga o passo a passo abaixo:

https://kubernetes.io/docs/tasks/configure-pod-container/assign-pods-nodes/

OBS: Você vai precisar utilizar o nome da label e o valor dela como parametro no Kubecost, então, tente utilizar algo como lifecyle:Ec2Spot ou algo do tipo.

# 3. Criando um DataFeed em seu ambiente AWS

Após isso, precisamos criar um Feed de Dados das instancias Spot de seu ambiente AWS.

Podemos fazer isso seguindo o link abaixo:

https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/spot-data-feeds.html

OBS: Importante garantir que as permissões do S3 estejam conforme a orientação da AWS no link acima

Para inscrever a subscrição, utilizamos o comando: 

aws ec2 create-spot-datafeed-subscription --bucket myawsbucket [--prefix myprefix]

Devemos ter uma saída como o exemplo abaixo:

{
    "SpotDatafeedSubscription": {
        "OwnerId": "111122223333",
        "Prefix": "myprefix",
        "Bucket": "myawsbucket",
        "State": "Active"
    }
}

O datafeed demora cerca de 1 hora para ser criado em seu bucket.

OBS: Guarde os valores da saída, utilizaremos esses parametros no Kubecost.

# 4. Configurando o Kubecost para monitorar os valores Spots 

Nas configurações de Instancias SPOTs do Kubecost, preencha os seguintes parametros:

Node label for spot instances: Nome da label que identifica o nó Spot.
Node label value for spot instances: Valor da label que identifica o nó Spot.
Spot data bucket name: Nome do bucket criado para o datafeed.
Spot data prefix: Nome do prefix criado para o datafeed.
Spot data region: Região onde o bucket foi criado.
AWS Account ID: Uma Account ID que possua permissão para acessar esse Bucket.
Service key name: Nome do Key Pair.
Service key secret: Service Key.

Pronto!

Após isso , seu kubecost utilizará o datafeed criado para calcular os valores do seu nó.


