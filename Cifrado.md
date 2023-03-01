# Algoritmo-de-Cifrado-Topicos-Avanzados-de-Programacion
Algoritmo de cifrado simple, dividido en dos, en primer lugar desplazamiento de tres posiciones, y en segundo cada línea debe invertirse.
package cifradophanel;
import java.io.IOException;
import java.awt.BorderLayout;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JTextArea;
import javax.swing.JTextField;
import java.awt.event.ActionListener;
import java.awt.event.ActionEvent;
import java.io.File;
import java.io.FileWriter;
import java.util.Scanner;
import javax.swing.JOptionPane;
public class CifradoPhanel {
    public static void main(String[] args) {
    CifradoPhanel ventana = new CifradoPhanel(); 
      ventana.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
      ventana.setResizable(false);
      ventana.setSize(500, 200); 
      ventana.setVisible(true);
    }
    private final BorderLayout layout;
    public final JTextField txtFile; 
    private final JTextArea text;
    private final JButton encryptJButton;
    private final JButton btnSaveFile;
    CifradoPhanel(){
    	super("Testing cipher");
         File f = new File("c:\\temp");
        if (f.mkdir()) {
            System.out.println("ya la creamos");
        }
        layout = new BorderLayout(20, 10);
        setLayout(layout);
        JPanel fileJPanel = new JPanel();
        JLabel lblFile = new JLabel( "Archivo de texto:" );
        fileJPanel.add(lblFile);
        txtFile = new JTextField(20);
        fileJPanel.add(txtFile);
        add(fileJPanel, BorderLayout.NORTH);
        text = new JTextArea(10,15);
    	add(text, BorderLayout.CENTER);
        JPanel buttonJPanel = new JPanel();
        buttonJPanel.setLayout(new BorderLayout());
        encryptJButton = new JButton("Encriptar");
    	buttonJPanel.add(encryptJButton, BorderLayout.NORTH);
        btnSaveFile = new JButton("leer Archivo");
        buttonJPanel.add(btnSaveFile,BorderLayout.AFTER_LAST_LINE);
        add(buttonJPanel, BorderLayout.EAST);
        txtFile.addActionListener(new txtAction()); 
        btnEncriptar e = new btnEncriptar();
        encryptJButton.addActionListener(e);
        btnLeerArchivo c = new btnLeerArchivo();
        btnSaveFile.addActionListener(c);
        }
        class txtAction implements ActionListener{
    	@Override
          public void actionPerformed(ActionEvent event)
          {
              text.setText(event.getActionCommand());
          }
          
    }
    class btnEncriptar implements ActionListener{
        @Override
            public void actionPerformed(ActionEvent event){
                String encript = txtFile.getText();
                if ( encript.isEmpty()){
                 JOptionPane.showMessageDialog(null, "ingresa texto por favor");
                }else{
                String textoEncriptado = encriptar(encript);
                text.setText(textoEncriptado);    
                    Crear(textoEncriptado);
                }
            }
    }
    class btnLeerArchivo implements ActionListener{
    @Override
        public void actionPerformed(ActionEvent event){
         leerArchivo();
        }
    }
    String encriptar(String s){
        char[] convert = new char[40];
        char[] lugar = new char[40];
        StringBuffer ec = new StringBuffer();
        String codigo= s;
        String palabra = codigo.trim();
        float aux = palabra.length();
        for (int i = 0; i < aux; i++) {
            char caracter = palabra.charAt(i);
            int ascii = (int)caracter+3;
            char codi = (char)ascii;
            convert[i] = codi;
        }
        for (int i = 1; i <= Math.round((aux/2)-.1); i++) {
            int num = (int)(aux)-i;
            lugar[i] = convert[num];
        }
        for (int i = 1; i <= Math.round((aux/2)); i++) {
            int auxChar = Math.round((aux/2)) - i;
            
            char caracter = convert[auxChar];
            int ascii = (int)caracter-1;
            char codi = (char)ascii;
            int aux1 = (int) Math.round((aux/2)-.1)+i;
            lugar[aux1] = codi;
        }
        for (int i = 0; i <= aux; i++) {
            ec = ec.append(lugar[i]);
        }
        return ec.toString();
    }
    public void Crear (String contenido ){
                try {
            String ruta = "C:\\temp\\Encriptamiento.txt";
            File file = new File(ruta);
            if (!file.exists()) {
                file.createNewFile();
            }
            FileWriter fw = new FileWriter(file,true);
            fw.write(contenido+"\r\n");
            fw.close();
        } catch (IOException e) {
            JOptionPane.showMessageDialog(null, e.toString());
        }
                 JOptionPane.showMessageDialog(null, "texto almacenado");
    }
    public void leerArchivo() {
        String textofinal ="";
      try {
          try (Scanner input = new Scanner(new File("C:\\temp\\Encriptamiento.txt"))) {
              while (input.hasNextLine()) {
                  String line  = input.nextLine();
                  System.out.println(line);
                 textofinal += line+"\r\n";
              }
               text.setText(textofinal);
          }
        } catch (Exception ex) {
              JOptionPane.showMessageDialog(null, ex.toString());
        }  
    }
}
