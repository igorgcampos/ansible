# 1 - Para rodar uma playbook pedindo a senha do vault, basta colocar o paramentro --ask-vault-pass e tambem nao esquecer do parametro -kK

# 2 - Para editar o arquivo de inventario do ansible que está criptografado:

	2.1 -> ansible-vault edit hosts

	  2.2 -> colocar a senha


# 3 - Para criptografar o arquivo hosts

	3.1 -> Entrar no diretorio

	  3.1.1 -> cd /etc/ansible/hosts

	  3.1.2 -> digitar o comando "ansible-vault encrypt hosts"


# 4 - Limit the ansible plabook to run in one single host

        4.1 -> Limit to one host

          4.1.2 -> ansible-playbook <playbook_name>.yml --limit "host"


# 5 - Troubleshooting para conectar em host windows

	5 -> Caso dê a mensagem "Connection refused" verificar no host se as portas do WinRm estão abertas

	  5.1 -> winrm enumerate winrm/config/Listener

	5.2 -> Se o servico nao estiver rodando, na porta 5985 e 5986, executar o script abaixo.

[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
$url = "https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1"
$file = "$env:temp\ConfigureRemotingForAnsible.ps1"

(New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)

powershell.exe -ExecutionPolicy ByPass -File $file

 	5.3 -> Se após executar o script ainda não tenha exito na conexao com o host. Desabilitar firewall do Windows

	  5.3.1 -> "Netsh Advfirewall show allprofiles" << Verificar se o firewall está ativado.

	  5.3.2 -> "NetSh Advfirewall set allprofiles state off" << Desabilita o firewall da maquina.


# 6 - Rodar uma playbook passando o argumento para colocar a senha do ansible vault

	6.1 -> Em host windows
          6.1.2 -> sudo ansible-playbook <nome_da_playbook.yml> --ask-vault-pass
	
	6.2 -> Em host linux
	  6.2.1 -> sudo ansible-playbook <nome_da_playbook.yml> --kK --ask-vault-pass # Paramentro "-kK" é para rodar em sudo
