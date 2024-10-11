# 이미지 빌드 과정에서 애플리케이션 빌드를 포함하여 작성하는 경우

# 빌드 과정에서 컴파일이 필요하기 때문에 jdk로 설치
FROM openjdk:17-jdk-slim AS builder

# 컨테이너 내부 디렉토리 경로
WORKDIR /usr/src/app

COPY gradlew .
COPY gradle gradle
COPY build.gradle .
COPY settings.gradle .

# 의존성만 먼저 다운로드하여 캐시 레이어 활용
RUN chmod +x gradlew

# RUN
RUN ./gradlew dependencies --no-daemon

COPY src src

# 빌드 수행
RUN ./gradlew build -x test --no-daemon

# Application 실행에 관련된 스크립트 --------------------------------
FROM openjdk:17-jdk-alpine

WORKDIR /app

COPY --from=builder /usr/src/app/build/libs/*.jar app.jar

ENTRYPOINT [ "java", "-jar", "/app/app.jar" ]