package org.firstinspires.ftc.teamcode;

import com.qualcomm.robotcore.eventloop.opmode.LinearOpMode;
import com.qualcomm.robotcore.eventloop.opmode.TeleOp;
import com.qualcomm.robotcore.hardware.DcMotor;
import com.qualcomm.robotcore.hardware.Servo;
import java.util.List;
import org.firstinspires.ftc.robotcore.external.JavaUtil;
import org.firstinspires.ftc.robotcore.external.hardware.camera.BuiltinCameraDirection;
import org.firstinspires.ftc.robotcore.external.hardware.camera.WebcamName;
import org.firstinspires.ftc.robotcore.external.tfod.Recognition;
import org.firstinspires.ftc.vision.VisionPortal;
import org.firstinspires.ftc.vision.tfod.TfodProcessor;

@TeleOp(name = "VisionObject (Blocks to Java)")
public class VisionObject extends LinearOpMode {

  private DcMotor LBMotor;
  private DcMotor LFMotor;
  private Servo FS;
  private DcMotor RBMotor;
  private DcMotor RFMotor;

  boolean USE_WEBCAM;
  TfodProcessor myTfodProcessor;
  VisionPortal myVisionPortal;
  float x;

  /**
   * This function is executed when this OpMode is selected from the Driver Station.
   */
  @Override
  public void runOpMode() {
    int ObjectDetected;

    LBMotor = hardwareMap.get(DcMotor.class, "LB Motor");
    LFMotor = hardwareMap.get(DcMotor.class, "LF Motor");
    FS = hardwareMap.get(Servo.class, "FS");
    RBMotor = hardwareMap.get(DcMotor.class, "RB Motor");
    RFMotor = hardwareMap.get(DcMotor.class, "RF Motor");

    // This 2023-2024 OpMode illustrates the basics of TensorFlow Object Detection.
    USE_WEBCAM = true;
    // Initialize TFOD before waitForStart.
    initTfod();
    LBMotor.setDirection(DcMotor.Direction.REVERSE);
    LFMotor.setDirection(DcMotor.Direction.REVERSE);
    FS.setDirection(Servo.Direction.REVERSE);
    FS.setPosition(0);
    // Wait for the match to begin.
    telemetry.addData("DS preview on/off", "3 dots, Camera Stream");
    telemetry.addData(">", "Touch Play to start OpMode");
    telemetry.update();
    waitForStart();
    if (opModeIsActive()) {
      // Put run blocks here.
      LBMotor.setPower(0.3);
      LFMotor.setPower(0.3);
      RBMotor.setPower(0.3);
      RFMotor.setPower(0.3);
      sleep(1525);
      LBMotor.setPower(0);
      LFMotor.setPower(0);
      RBMotor.setPower(0);
      RFMotor.setPower(0);
      for (int count = 0; count < 3; count++) {
        // Put loop blocks here.
        telemetryTfod();
        // Push telemetry to the Driver Station.
        telemetry.update();
        if (gamepad1.dpad_down) {
          // Temporarily stop the streaming session.
          myVisionPortal.stopStreaming();
        } else if (gamepad1.dpad_up) {
          // Resume the streaming session if previously stopped.
          myVisionPortal.resumeStreaming();
        }
        // Share the CPU.
        sleep(20);
        if (x > 0) {
          ObjectDetected = 1;
        } else {
          sleep(3000);
          ObjectDetected = 0;
        }
        telemetry.addLine(" ");
        telemetry.addData("ObjT/F", ObjectDetected);
        telemetry.update();
      }
      sleep(2000);
      if (ObjectDetected == 1) {
        FS.setPosition(1);
      } else {
        sleep(8000);
        LBMotor.setPower(0.3);
        RFMotor.setPower(0.3);
        sleep(2000);
        LBMotor.setPower(0);
        RFMotor.setPower(0);
        if (ObjectDetected == 1) {
          FS.setPosition(1);
        } else {
          sleep(8000);
          LBMotor.setPower(0.3);
          RFMotor.setPower(0.3);
          sleep(2000);
          LBMotor.setPower(0);
          RFMotor.setPower(0);
        }
      }
    }
    while (opModeIsActive()) {
      // Put loop blocks here.
      telemetryTfod();
      // Push telemetry to the Driver Station.
      telemetry.update();
      if (gamepad1.dpad_down) {
        // Temporarily stop the streaming session.
        myVisionPortal.stopStreaming();
      } else if (gamepad1.dpad_up) {
        // Resume the streaming session if previously stopped.
        myVisionPortal.resumeStreaming();
      }
      // Share the CPU.
      sleep(20);
      if (x > 0) {
        ObjectDetected = 1;
      } else {
        sleep(3000);
        ObjectDetected = 0;
      }
      telemetry.addLine(" ");
      telemetry.addData("ObjT/F", ObjectDetected);
      telemetry.update();
    }
  }

  /**
   * Initialize TensorFlow Object Detection.
   */
  private void initTfod() {
    // First, create a TfodProcessor.
    myTfodProcessor = TfodProcessor.easyCreateWithDefaults();
    // Next, create a VisionPortal.
    if (USE_WEBCAM) {
      myVisionPortal = VisionPortal.easyCreateWithDefaults(hardwareMap.get(WebcamName.class, "Webcam 1"), myTfodProcessor);
    } else {
      myVisionPortal = VisionPortal.easyCreateWithDefaults(BuiltinCameraDirection.BACK, myTfodProcessor);
    }
  }

  /**
   * Display info (using telemetry) for a detected object
   */
  private void telemetryTfod() {
    List<Recognition> myTfodRecognitions;
    Recognition myTfodRecognition;
    float y;

    // Get a list of recognitions from TFOD.
    myTfodRecognitions = myTfodProcessor.getRecognitions();
    telemetry.addData("# Objects Detected", JavaUtil.listLength(myTfodRecognitions));
    // Iterate through list and call a function to display info for each recognized object.
    for (Recognition myTfodRecognition_item : myTfodRecognitions) {
      myTfodRecognition = myTfodRecognition_item;
      // Display info about the recognition.
      telemetry.addLine("");
      // Display label and confidence.
      // Display the label and confidence for the recognition.
      telemetry.addData("Image", myTfodRecognition.getLabel() + " (" + JavaUtil.formatNumber(myTfodRecognition.getConfidence() * 100, 0) + " % Conf.)");
      // Display position.
      x = (myTfodRecognition.getLeft() + myTfodRecognition.getRight()) / 2;
      y = (myTfodRecognition.getTop() + myTfodRecognition.getBottom()) / 2;
      // Display the position of the center of the detection boundary for the recognition
      telemetry.addData("- Position", JavaUtil.formatNumber(x, 0) + ", " + JavaUtil.formatNumber(y, 0));
      // Display size
      // Display the size of detection boundary for the recognition
      telemetry.addData("- Size", JavaUtil.formatNumber(myTfodRecognition.getWidth(), 0) + " x " + JavaUtil.formatNumber(myTfodRecognition.getHeight(), 0));
    }
  }
}
