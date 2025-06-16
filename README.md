# chatReact
chatReact-demo
package com.cykj.ui.sample.ble;

import android.os.Build;
import android.os.Environment;
import android.text.TextUtils;
import android.util.Log;

import com.clj.fastble.utils.HexUtil;

import java.io.File;

public class CYHexDataTool {

    public static int getValidLength(String dataString) {
        int validLength = 0;
        if (dataString.length() >= 8) {
            String parti = dataString.substring(4, 8);
            validLength = Integer.valueOf(parti, 16);
        }
        return validLength;
    }

    public static String getValidDataString(String dataString, int validLength) {
        int ValidDataIndex = 14 + validLength * 2;
        if (dataString.length() >= ValidDataIndex) {
            return dataString.substring(14, ValidDataIndex);
        }
        return "";
    }

    public static boolean judgeValidDataLength(String dataString, int validLength) {
        int validTotalLength = validLength * 2 + 16;
        if (dataString.length() == validTotalLength) {
            return true;
        }
        return false;
    }

    public static boolean judgeNoRespondData(byte[] data) {
        String strRece = HexUtil.formatHexString(data);
        int validLength = CYHexDataTool.getValidLength(strRece);
        if (validLength == 3) {
            String parti2 = getValidDataString(strRece,validLength);
            if (!TextUtils.isEmpty(parti2) && parti2.toUpperCase().equals("00FE00")) {
                return true;
            }
        }
        return false;
    }

    //
    public static boolean judgeValidHeadDataLength(String dataString) {
        int index = dataString.toUpperCase().indexOf("AA55");
        if (index == 0) {
            return true;
        }
        return false;
    }


    public static String getOBDLocalPath() {
        return getBaseLocalPath("OBDII_EOBD");
    }

    public static String getHANDHELDLocalPath() {
        return getBaseLocalPath("HANDHELD");
    }

    private static String getBaseLocalPath(String target) {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.R) {
            if (Environment.isExternalStorageManager()) {
                File externalStorageDir = Environment.getExternalStorageDirectory();
                File documentsFolder = new File(externalStorageDir, "Documents");
                File cyFolder = new File(documentsFolder, "CHAOYUE");
                File hsFolder = new File(cyFolder, "HS32110281");
                File finalFolder = new File(hsFolder, target);
                String testFolderPath = finalFolder.getAbsolutePath();
                // testFolderPath就是目标TEST文件夹的路径
                Log.d("Donglin Debug", "getLocalPath0: " + testFolderPath);
                return testFolderPath+"/";
            }
        } else {
            File documentsFolder = Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_DOCUMENTS);
            File cyFolder = new File(documentsFolder, "CHAOYUE");
            File hsFolder = new File(cyFolder, "HS32110281");
            File finalFolder = new File(hsFolder, target);
            String testFolderPath = finalFolder.getAbsolutePath();
            // testFolderPath就是目标TEST文件夹的路径
            Log.d("Donglin Debug", "getLocalPath1: " + testFolderPath);
            return testFolderPath+"/";
        }
        return "";
    }

    public static String getLogLocalPath() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.R) {
            if (Environment.isExternalStorageManager()) {
                File externalStorageDir = Environment.getExternalStorageDirectory();
                File documentsFolder = new File(externalStorageDir, "Documents");
                File cyFolder = new File(documentsFolder, "CHAOYUE");
                File hsFolder = new File(cyFolder, "HS32110281");
                File finalFolder = new File(hsFolder, "log");
                String testFolderPath = finalFolder.getAbsolutePath();
                // testFolderPath就是目标TEST文件夹的路径
                Log.d("Donglin Debug", "getLocalPath0: " + testFolderPath);
                return testFolderPath+"/";
            }
        } else {
            File documentsFolder = Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_DOCUMENTS);
            File cyFolder = new File(documentsFolder, "CHAOYUE");
            File hsFolder = new File(cyFolder, "HS32110281");
            File finalFolder = new File(hsFolder, "log");
            String testFolderPath = finalFolder.getAbsolutePath();
            // testFolderPath就是目标TEST文件夹的路径
            Log.d("Donglin Debug", "getLocalPath1: " + testFolderPath);
            return testFolderPath+"/";
        }
        return "";
    }
}
