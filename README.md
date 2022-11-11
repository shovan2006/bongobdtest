package newpackage;

import java.awt.AWTException;
import java.awt.Robot;
import java.awt.event.InputEvent;
import java.awt.event.KeyEvent;
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Random;
import java.util.Date;
import java.util.HashMap;
import java.util.Iterator;
import java.util.List;
import java.util.Map;
import java.util.Set;
import java.util.concurrent.TimeUnit;
import org.apache.poi.ss.usermodel.Cell;
import org.apache.poi.ss.usermodel.Row;
import org.apache.poi.ss.usermodel.Sheet;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;
import org.openqa.selenium.Alert;
import org.openqa.selenium.By;
import org.openqa.selenium.JavascriptExecutor;
import org.openqa.selenium.Keys;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.openqa.selenium.interactions.Actions;
import org.openqa.selenium.support.ui.Select;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.openqa.selenium.support.ui.ExpectedConditions;
import java.util.NoSuchElementException;
import org.testng.annotations.Test;
import org.testng.annotations.BeforeSuite;
import org.testng.annotations.AfterSuite;

@SuppressWarnings("unused")

public class MyClassDDT {	 
	static WebDriver driver;
	WebElement element;
	static String localDir = System.getProperty("user.dir");
	public String baseUrl = "https://www.saucedemo.com/";
	
		
	@BeforeSuite
	public void beforeSuite() throws InterruptedException {
		
		
		// ----------Flash Player setup-------------------
		ChromeOptions options = new ChromeOptions();
		// Disable extensions and hide infobars
		options.addArguments("--disable-extensions");
		options.addArguments("disable-infobars");

		Map<String, Object> prefs = new HashMap<>();

		// Enable Flash
		prefs.put("profile.default_content_setting_values.plugins", 1);
		prefs.put(
				"profile.content_settings.plugin_whitelist.adobe-flash-player",	1);
		prefs.put(
				"profile.content_settings.exceptions.plugins.*,*.per_resource.adobe-flash-player", 1);

		// Hide save credentials prompt
		prefs.put("credentials_enable_service", false);
		prefs.put("profile.password_manager_enabled", false);
		options.setExperimentalOption("prefs", prefs);

		// --------End Flash Player Setup-----

		// Set Browser
		// ******************************************************************************************************************
		//System.setProperty("webdriver.chrome.driver","C:\\Users\\admin\\eclipse\\chromedriver.exe");
    	
		driver = new ChromeDriver(options);
		//driver = new FirefoxDriver();
		
		driver.manage().timeouts().implicitlyWait(120, TimeUnit.SECONDS);
		driver.manage().window().maximize();
		Thread.sleep(2000);

	}
	
	@Test(priority = 0)
	public void TC0_Launch_application() throws IOException, InterruptedException {
		// open test URL
		driver.get(baseUrl);
		Thread.sleep(2000);
		
	}
		
	@Test(priority = 1) // Data Driven Test (DDT) Login & Logout to CoinBet
	public void TC1_DDT() throws  InterruptedException, IOException, AWTException {
						
		// ------- Login ------- \\
		
		File src=new File("C:\\Users\\admin\\eclipse-workspace\\newproject\\InputTestDataExcel\\emplogin.xlsx");

		  // Load the file

		  FileInputStream fis=new FileInputStream(src);

		   // load the workbook

		   XSSFWorkbook wb=new XSSFWorkbook(fis);

		  // get the sheet which you want to modify or create
		   
		   Sheet ExlSheet = wb.getSheet("emplogin");
		  // XSSFSheet sh1= wb.getSheetAt(0);
		   int FristRow=ExlSheet.getFirstRowNum();
		   int RowCount = ExlSheet.getLastRowNum();
		   int TCount= RowCount-FristRow;
		   System.out.println("Row Count: "+RowCount);
		   System.out.println("First Row: "+FristRow);
		   System.out.println("Total Row: "+TCount);
		   
		   System.out.println("///////////////////////////////////////////////////////");
		   Row row;
		   		   			
		   for (int i = 1; i <= TCount; i++) { 

			   row = ExlSheet.getRow(i);
			        
			   //Create a loop to print cell values in a row
			   System.out.println("Total Column: "+row.getLastCellNum());  
			      
			   for (int j = 0; j < row.getLastCellNum(); j++) {
			        	
			   System.out.println("Test Column: "+j);
			   
			    	switch (j){
			        		
			        	case 0:
			        			
			        		System.out.println("Username: " + row.getCell(j)); // read Username from XL file
			        		Thread.sleep(2000);
			        		driver.findElement(By.xpath("//input[@type='text']")).clear();
					        driver.findElement(By.xpath("//input[@type='text']")).sendKeys(row.getCell(j).getStringCellValue());
					        Thread.sleep(2000);
			        		break;
			        		
			        	case 1:
			        			
			        		System.out.println("Password: " + row.getCell(j)); // read password from XL file
			        		Thread.sleep(2000);
			        		driver.findElement(By.xpath("//input[@type='password']")).clear();
					        driver.findElement(By.xpath("//input[@type='password']")).sendKeys(row.getCell(j).getStringCellValue());
					        Thread.sleep(2000);
				        	break;
			        		
			        	default: 
		        			System.out.println("<-----Default Values------>");
		        				
			        }	
			        
			    }	
		
		
		//Login Button
		driver.findElement(By.name("login-button")).click();
		Thread.sleep(2000); 
		  
		List<WebElement> dashboard = driver.findElements(By.className("title"));
		Thread.sleep(2000);
		if(dashboard.isEmpty()) {
			
			System.out.println("User" + i + ": Login Failed!");
			Thread.sleep(10000);
		}
		else {
		
			String dashboard1 = dashboard.get(0).getText();
			System.out.println(dashboard1 + i + ": User" + i + " Login Success!");    
		
				
		// ------- Select 3 items ------- \\
		
		List<WebElement> li= driver.findElements(By.name("add-to-cart-sauce-labs-backpack"));
		li.size() = 3;		
	    	for(int i=1;i<=li.size();i++)
        	{
	    		driver.findElement(By.xpath("/html/body/div/div/div/div[2]/div/div/div/div["+i+"]/div[2]/div[2]/button")).click(); //Click Add to cart button
			Thread.sleep(3000);
       		}
		
		// ------- Logout ------- \\
        
		Thread.sleep(2000);
		driver.findElement(By.id("react-burger-menu-btn")).click();
		Thread.sleep(3000); 
        	driver.findElement(By.linkText("logout")).click();
        	Thread.sleep(10000);
        		
        	driver.manage().deleteAllCookies();     	 
		Thread.sleep(2000);
		}
        	System.out.println("///////////////////////////////////////////////////////");
			   
		}
		wb.close();
	
	}
	
	
	@Test(priority = 2) // End of Data Driven Test (DDT)
	public void TC2_EOT() throws InterruptedException, IOException, AWTException{
		
        System.out.println("DDT Has been Completed. Thank You!");
		
	}
		
	@AfterSuite	// Close browser
	public void afterSuite() {
	
		driver.close();	
	
	}

}
