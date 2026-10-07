## Lösung

1. Python 3.14 per dnf installieren: `sudo dnf install python3.14`
   Test: `rpm -qf /usr/bin/python3.14` → `python3.14-3.14.5-1.el9_8.x86_64`
2. venv für Ansible anlegen: `sudo /usr/bin/python3.14 -m venv /opt/ansible`
3. Ansible 2.21 im venv installieren: `sudo /opt/ansible/bin/pip install 'ansible-core==2.21.*'`
4. Rechte für alle Benutzer freigeben (nötig wegen restriktiver umask): `sudo chmod -R a+rX /opt/ansible`
5. Verweise anlegen, damit `ansible` für alle Benutzer ohne Aktivierung 2.21 startet: `sudo ln -s /opt/ansible/bin/ansible* /usr/local/bin/`
6. Prüfen (neue Shell oder `hash -r`): `type -a ansible-playbook` zeigt /usr/local/bin zuerst, `ansible --version` zeigt `core 2.21.x` mit Python 3.14

### Hinweise
- Updates: `sudo /opt/ansible/bin/pip install --upgrade 'ansible-core==2.21.*'`, danach Schritt 4 wiederholen
- Das RPM mit 2.14 bleibt unter /usr/bin als Fallback, `sudo ansible` nutzt weiterhin 2.14
- Rollback: `sudo find /usr/local/bin -lname '/opt/ansible/bin/*' -delete`