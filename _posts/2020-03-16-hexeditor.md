
      ---
layout: post
title: HexEditor
date: 2020-03-16 17:25:20 +0300
description: HexEditor
img: HexEditor.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [HexEditor]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java

public static class Hex {
	private final static char[] hex_digits = {
		'0', '1', '2', '3', '4', '5', '6', '7', 
		'8', '9', 'a', 'b', 'c', 'd', 'e', 'f'
	};
	public static char[] bytesToHex(byte[] bytes) {
		int bytes_length = bytes.length;
		char[] hex_form = new char[bytes_length*2];
		for( int i = 0, j = 0; i<bytes_length; i++, j=i*2 ) {
			hex_form[j] 	= hex_digits[ (bytes[i] & 0xf0 ) >>> 4 ];
			hex_form[j+1]  	= hex_digits[ (bytes[i] & 0x0f) ];
		}
		return hex_form;
	}
	public static byte[] hexToBytes(char[] hex_chars) {
		int hex_length = hex_chars.length;
		int byte_length = hex_length/2;
		byte[] byte_form = new byte[byte_length];
		for(int i = 0, j = 0; i<byte_length; i++, j=i*2) {
			int most_sig  =	hexToDec(hex_chars[j], 1);
			int least_sig = hexToDec(hex_chars[j+1], 0);
			byte_form[i] = (byte)(most_sig + least_sig);
		}
		return byte_form;
	}
	public static int hexToDec(char hex_char, int position) {
		return Character.digit(hex_char, 16) << position*4; 
	}
}


public static class HexEditor {
	public String file_hex_string;
	public String filepath;
	public java.io.File file;
	public HexEditor(String filepath) throws java.io.IOException {
		open(filepath);
	}
	public void open(String filepath) throws java.io.IOException {		
		this.filepath = filepath;
		file = new java.io.File(filepath);
		byte[] file_bytes = new byte[(int)file.length()];
		java.io.FileInputStream fis 	= new java.io.FileInputStream(file);
		int  bytes_read = 0;
		do {
			bytes_read = fis.read(file_bytes);
		}
		while (bytes_read != -1);
		file_hex_string = new String(Hex.bytesToHex(file_bytes));
		fis.close();
	}
	public void saveAs(String filepath) throws java.io.IOException, java.io.FileNotFoundException {
		byte[] file_hex_bytes = Hex.hexToBytes(file_hex_string.toCharArray());
		java.io.FileOutputStream file_save = new java.io.FileOutputStream(filepath);
		file_save.write(file_hex_bytes);
	}
	public void save() throws java.io.IOException, java.io.FileNotFoundException {
		saveAs(filepath);
	}
	public void delete() {
		file.delete();	
	}
	public void replace(CharSequence match, CharSequence replacement) {
		file_hex_string = file_hex_string.replace(match, replacement);
	}
	public void regexReplace(String regex, String replacement) {
		file_hex_string = file_hex_string.replaceAll(regex, replacement);

	}
	public void replacePosition(int position, char replacement) {
		file_hex_string = file_hex_string.substring(0, position) + replacement + file_hex_string.substring(position + 1);
	}
}


public static class ReadBytes {
	public static void Read(String _file) {
		java.io.File file = new java.io.File(_file);
		java.io.FileInputStream fin = null;
		try {
			fin = new java.io.FileInputStream(file);
			byte fileContent[] = new byte[(int)file.length()];
			fin.read(fileContent);
			String s = new String(fileContent);
			System.out.println("File content: " + s);
		}
		catch (java.io.FileNotFoundException e) {
			System.out.println("File not found" + e);
		}
		catch (java.io.IOException e) {
			System.out.println("Exception while reading file " + e);
		}
		finally {
			try {
				if (fin != null) {
					fin.close();
				}
			}
			catch (java.io.IOException e) {
				System.out.println("Error while closing stream: " + e);
			}
		}
	}
}
 
 



//USAGE
----------------------------
//To open a binary file:
HexEditor HexEditor = new HexEditor( "/path/to/binary" );

//For simple string replacements:
HexEditor.replace( "4a", "5b");

//For regex replacements:
HexEditor.regexReplace( "5b", "6c");

//For positional replacements:
HexEditor.replacePosition( 2, 'f');

//Saving:
HexEditor.save();
HexEditor.saveAs( "/path/to/newbinary" );

//Deletion:
HexEditor.delete();
--------------------------------
```
      