# emails
 Script para enviar e-mails em massa com anexos em pdf
 
 
 
 
 
import smtplib
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from email.mime.base import MIMEBase
from email import encoders


msg = MIMEMultipart()

##    Input para login para envio do  E-mail
msg['From'] = input("E-mail : ")
print(msg['From'])
password = input("Sua senha : ")

## Corpo do E-mail
msg['Subject'] = "Assunto"
body = " Mensagem "

msg.attach(MIMEText(body,'plain'))

## Anexo
attachment = open('Caminho_do_arquivo','rb')

part = MIMEBase('application','pdf')
part.set_payload((attachment).read())
encoders.encode_base64(part)
part.add_header( 'Content-Disposition' , 'attachment' , filename = 'Nome_do_Arquivo.pdf')

msg.attach(part)
attachment.close()

## lista de e-mail, com um interador para seu indice.

List_Emails = ['ajuel.dka@gmail.com', 'lojas.jos@gmail.com']
Ilist = 0


# Configuração do servidor SMTP do remetende
try:
    server = smtplib.SMTP('smtp.gmail.com', 587)
    server.starttls()
    text = msg.as_string()
    server.login(msg['From'], password)
    Check = True

except:
    Check = False

while Check:
    # Envio do e-mail
    server.sendmail(msg['From'],List_Emails[Ilist],text)

    Ilist += 1
    if Ilist >= len(List_Emails):
        print("Emails enviados")
        break


else:
    print(" Erro ao fazer login.\n Por Favor reinicie o programa. ")
# Saida do server
server.quit()
