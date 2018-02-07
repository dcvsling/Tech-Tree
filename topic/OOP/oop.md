## 物件導向程式開發思維

---

物件導向推出這麼多年之後才終於出現所謂的開發原則，這勢必有它的原因，由此可知在這之前ＯＯＰ應該要如何撰寫我們都還處於瞎子摸象階段，或是百家爭鳴階段，也所以才會有所謂的麵條是程式以及程式碼的Bad Smell．我們就從那不堪回首的程式碼年代開始說起吧．



## 過去的Bad Smell

---

在過去我們的程式撰寫方式是，依照預期運作流程去沿線去進行開發，

ex：需求為，將上傳的檔案加以分析後轉成PDF，Execl兩種匯出格式，並將資料留存於Db

依照過去的開發模式，毫無考慮的就是將FileUpload元件上傳的Stream轉為可分析的Data

然後再使用第三方API轉出Excel和PDF，最後將資料存進Database

大概會像是這樣子

```
public class UploadFileProcess
{
    private DataTable _data;
    public UploadFileProcess(FileStream stream)
    {
        _data = SerializeStream(stream);
    }
    
    private void SerializeStream(FileStream stream)
    {
        // do serialize
        return result;
    }
    
    public string ExportToExcel(string Path)
    {
        string ErrMsg = String.Empty;
        try{
            // do export to Excel
        }
        catch(Exception ex)
        {
            ErrMsg = ex.ToString();
            throw new Exception("export excel fail => " + ErrMsg);
        }
        finally
        {
            return ErrMsg;
        }
    }
    
    public string ExportToPDF(string Path)
    {
        string ErrMsg = String.Empty;
        try{
            // do export to PDF
        }
        catch(Exception ex)
        {
            ErrMsg = ex.ToString();
            throw new Exception("export pdf fail => " + ErrMsg);
        }
        finally
        {
            return ErrMsg;
        }
    }
    
    public string SaveToDb()
    {
        string ErrMsg = String.Empty;
        try{
            // Save To Db
        }
        catch(Exception ex)
        {
            ErrMsg = ex.ToString();
            throw new Exception("save Db fail => " + ErrMsg);
        }
        finally
        {
            return ErrMsg;
        }
    }
}
string ErrMsg = string.Empty;
var process = new UploadFileProcess(FileUpLoad.FileStream);
ErrMsg += process.ExportToExcel(@"D:\FileUploadExport\" + Guid.New().ToString());
ErrMsg += process.ExportToPDF(@"D:\FileUploadExport" + Guid.New().ToString());
ErrMsg += process.SaveToDb();
if(ErrMsg.Length != 0) throw new Exception(ErrMsg);    
```

這應該是我現階段能想到的最簡單最短且最糟糕的一段實踐這個需求的程式碼吧

先寫到這裡，讓看倌們想想看自己曾經寫過裡面的那些，以及現在仍在寫得有哪些，也可以順便想一想過去改變作法的理由

