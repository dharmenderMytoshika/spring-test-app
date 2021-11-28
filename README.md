# spring-test-app

To mock utility class and final class with mockito

if using gradle: build.gradle

testImplementation 'org.mockito:mockito-inline:2.13.0'

if using maven: pom.xml

<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-inline</artifactId>
    <version>2.13.0</version>
    <scope>test</scope>
</dependency>


code:

@SpringBootTest
public class DateExpirationTest {

	@Test
	public void isExpiredTest() throws NoSuchMethodException, SecurityException, IllegalAccessException, IllegalArgumentException, InvocationTargetException {
		Method method = DateExpirationService.class.getDeclaredMethod("isExpired", String.class);
		method.setAccessible(true);
		DateExpirationService dateExpirationService = new DateExpirationService();
		Boolean result = (boolean) method.invoke(dateExpirationService, "2021-11-22T17:35:29.0");
		assertEquals(true, result);
	}
}


import java.time.ZoneOffset;
import java.time.ZonedDateTime;
import java.time.format.DateTimeFormatter;
import java.time.format.DateTimeFormatterBuilder;

import org.springframework.stereotype.Service;

@Service
public class DateExpirationService {

	public static final DateTimeFormatter dateTimeFormatter = new DateTimeFormatterBuilder()
			.appendOptional(DateTimeFormatter.ofPattern("yyyy-MM-dd'T'HH:mm:ss.SSSSSSSSS"))
			.appendOptional(DateTimeFormatter.ofPattern("yyyy-MM-dd'T'HH:mm:ss.SSSSSSSS"))
			.appendOptional(DateTimeFormatter.ofPattern("yyyy-MM-dd'T'HH:mm:ss.SSSSSSS"))
			.appendOptional(DateTimeFormatter.ofPattern("yyyy-MM-dd'T'HH:mm:ss.SSSSSS"))
			.appendOptional(DateTimeFormatter.ofPattern("yyyy-MM-dd'T'HH:mm:ss.SSSSS"))
			.appendOptional(DateTimeFormatter.ofPattern("yyyy-MM-dd'T'HH:mm:ss.SSSS"))
			.appendOptional(DateTimeFormatter.ofPattern("yyyy-MM-dd'T'HH:mm:ss.SSS"))
			.appendOptional(DateTimeFormatter.ofPattern("yyyy-MM-dd'T'HH:mm:ss.SS"))
			.appendOptional(DateTimeFormatter.ofPattern("yyyy-MM-dd'T'HH:mm:ss.S"))
			.toFormatter();

    private static boolean isExpired(final String date) {
        final ZonedDateTime expiryDate = ZonedDateTime.parse(date, dateTimeFormatter.withZone(ZoneOffset.UTC)).withZoneSameInstant(ZoneOffset.UTC);
		System.out.println(expiryDate);
		System.out.println(ZonedDateTime.now(ZoneOffset.UTC));
        return expiryDate.isBefore(ZonedDateTime.now(ZoneOffset.UTC));
    }

	public static void main(String[] args) {

		System.out.println(isExpired("2021-11-20T17:35:29.0"));
	}
}
