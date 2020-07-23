package bongobd;

import java.awt.AWTException;
import java.awt.Robot;
import java.awt.event.KeyEvent;
import java.io.File;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.TimeUnit;
import org.openqa.selenium.By;
import org.openqa.selenium.Keys;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.interactions.Actions;
import org.openqa.selenium.support.ui.Select;
import org.testng.annotations.Test;
import org.testng.annotations.BeforeSuite;
import org.testng.annotations.AfterSuite;

public class Test {
	WebDriver driver;
	WebElement element;
	static String localDir = System.getProperty("user.dir");
	
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

		// -----------End Flash Player Setup-----

		// Set Browser
		// ******************************************************************************************************************//
		
		System.setProperty("webdriver.chrome.driver","C:\\Users\\shahadat.hossain1563\\eclipse\\chromedriver.exe");
    	
		driver = new ChromeDriver(options);

		/*
		 * System.setProperty("webdriver.gecko.driver",
		 * "C:\\Users\\shahadat.hossain1563\\eclipse\\geckodriver.exe"
		 * ); DesiredCapabilities capabilities=DesiredCapabilities.firefox();
		 * capabilities.setCapability("marionette", true); driver = new
		 * FirefoxDriver();
		 */

		driver.manage().timeouts().implicitlyWait(120, TimeUnit.SECONDS);
		driver.manage().window().maximize();
		Thread.sleep(2000);

	}

	@Test(priority = 1)
	public void TC1() throws IOException, InterruptedException {
		// open URL Bongobd
		driver.get("https://bongobd.com/");
		Thread.sleep(3000);

	}
	
	@Test(priority = 2)
	public void TC2() throws  InterruptedException, IOException, AWTException {
		Robot robot = new Robot();
		
		Thread.sleep(3000);
		
		// Click on "Movies" from the menu bar
		driver.findElement(By.linkText("Movies")).click(); 
		Thread.sleep(6000);
		
		// Scroll page down event by robot.
		robot.keyPress(KeyEvent.VK_PAGE_DOWN);
		
		// LOAD THE DETAILS PAGE OF THE FIRST MOVIE
		driver.findElement(By.xpath("//*[@id="root"]/div/div/div/div[3]/div/div[2]/div/div/div[2]/div/div/div/div/div[7]/div/div/div/a/div[2]")).click();
		Thread.sleep(10000);
		

		// Scroll page down event by robot.
		robot.keyPress(KeyEvent.VK_PAGE_DOWN);
	    	
		// PLAY/STOP THE MOVIE
		driver.findElement(By.xpath("//*[@id="vjs_video_3"]/div[4]/button[2]")).click();
	    	Thread.sleep(3000);
	    	 
	}
	
	@AfterSuite
	public void afterSuite() {
	}

}
