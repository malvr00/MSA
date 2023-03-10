# 키 생성

[ 예 시 ]<br />
1( private key 생성 ). keytool -genkeypair -alias apiEncryptionKey -keyalg RSA \
     -dname “CN=Kenneth Lee, OU=API Development, O=joneconsulting.co.kr, L=Seoul, C=KR” \
     -keypass “1q2w3e4r” -keystore apiEncryptionKey.jks -storepass “1q2w3e4r” 
     
2( 인증서 생성 ). keytool -export -alias apiEncryptionKey -keystore apiEncryptionKey.jks -rfc -file strustServer.cer

3( 다시 Key 변환 trustedCertEntry (public) ).keytool -import -alias strustServer -file strustServer.cer -keystore publicKey.jks
<br />
[ 도 움 말 ]
-genkeypair => public, private 동시 사용.
-alias => 항목 별칭
-keyalg => 알고리즘 방식
-dname => 서명정보
-keypass => 비밀번호
-keystore => 스토리 이름
-storepass => 비밀번호
