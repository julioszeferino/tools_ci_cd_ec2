1. Adicionar este trecho ao arquivo go.yml
```yaml
deploy_EC2:
    needs: build
    uses: ./.github/workflows/ec2.yml
    secrets: inherit
```

2. Criar o arquivo ec2.yml

> Devemos sempre tomar cuidado ao trazermos um artefato para a nossa rotina, já que se os arquivos do artefato tiverem o mesmo nome de um arquivo da aplicação ele pode substituir o arquivo, e o mesmo também pode ocorrer se trouxermos a aplicação depois do artefato.

> Então se tivermos um arquivo main.go dentro de um artefato e também no nosso repositório, com conteúdos diferentes, o último que chegar na nossa rotina irá substituir o primeiro.Então, se trouxermos a rotina com o actions/checkout@v3 e depois o artefato com `actions/download-artifact@v3.0.0`, a versão dele vai ser a que estará dentro da rotina.

> Outro ponto importante é que até o actions/checkout@v2 todos os arquivos eram deletados da pasta de trabalho durante a sua execução e isso foi levantado como uma vantagem, então para garantir, sempre que você precisar executar um código que precise do actions/checkout, dê preferência para executá-lo como a primeira tarefa.

3. Criar os secrets
- SSH_PRIVATE_KEY: basta colar no value o codigo da chave que realizamos download quando configuramos a VM
- REMOTE_HOST: No painel EC2 > Instancias > Copiar o Endereco IPv4 Publico
- REMOTE_USER: No painel EC2 > Instancias > Seleciona a instancia > Conectar > Aba Conexao de Instancia do EC2 > Nome do usuario
- 