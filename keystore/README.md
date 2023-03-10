# 키 생성

[ 예 시 ]<br />
1( private key 생성 ). keytool -genkeypair -alias apiEncryptionKey -keyalg RSA \
     -dname “CN=Kenneth Lee, OU=API Development, O=joneconsulting.co.kr, L=Seoul, C=KR” \
     -keypass “1q2w3e4r” -keystore apiEncryptionKey.jks -storepass “1q2w3e4r” 
     
2( 인증서 생성 ). keytool -export -alias apiEncryptionKey -keystore apiEncryptionKey.jks -rfc -file strustServer.cer

3( 다시 Key 변환 trustedCertEntry (public) ).keytool -import -alias strustServer -file strustServer.cer -keystore publicKey.jks
<br /><br />
[ 도 움 말 ]<br />
-genkeypair => public, private 동시 사용.<br />
-alias => 항목 별칭<br />
-keyalg => 알고리즘 방식<br />
-dname => 서명정보<br />
-keypass => 비밀번호<br />
-keystore => 스토리 이름<br />
-storepass => 비밀번호<br />
