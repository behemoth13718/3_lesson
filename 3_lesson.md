# 3_lesson

====================
метод подключения юзверя
Показываем предыдущий чат(сколько нужно строк)
======================
void connect() {
        Socket socket = null;
        try {
            socket = new Socket(tfIPAddress.getText(), Integer.parseInt(tfPort.getText()));
            try {
                FileReader fr = new FileReader("His.txt");
                BufferedReader br = new BufferedReader(fr);
                int lines =0;
                StringBuilder text= new StringBuilder();


                for (int i = 0; i <100 ; i++) {

                    String str = br.readLine();
                    lines++;
                    System.out.println(str + "\n");
                    text.append(str);
                    text.append('\n');
                }
                log.setText(text.toString());
                System.out.println(lines);
                br.close();
            } catch (IOException e0) {
                System.out.println("Файл не найден!");
            }
        } catch (IOException e) {
            log.append("Exception: " + e.getMessage());
        }
        socketThread = new SocketThread(this, "Client thread", socket);
    }


никак не получается вывести последние 100 строк, а не совокупность
===================================
метод отправляет сообщения и в нем ведется история чата
==================================================
  void sendMessage() {
        String msg = tfMessage.getText();
        String username = tfLogin.getText();
        if ("".equals(msg)) return;
        tfMessage.setText(null);
        tfMessage.requestFocusInWindow();
        socketThread.sendMessage(Messages.getTypeBroadcastShort(msg));
        try (FileWriter out = new FileWriter("His.txt", true)) {
        out.write(username + ": " + msg + "\n");
            out.flush();
        } catch (IOException e) {
            if (!shownIoErrors) {
                shownIoErrors = true;
                log.append("System: File write error\n");
                JOptionPane.showMessageDialog(this, "File write error", "Exception", JOptionPane.ERROR_MESSAGE);
            }
        }
    }
