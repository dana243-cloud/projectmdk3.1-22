package com.example.derpr2;

import javafx.fxml.FXML;
import javafx.scene.Group;
import javafx.scene.PerspectiveCamera;
import javafx.scene.input.MouseButton;
import javafx.scene.input.MouseEvent;
import javafx.scene.control.Slider;
import javafx.scene.input.ScrollEvent;
import javafx.scene.AmbientLight;
import javafx.scene.PointLight;
import javafx.scene.paint.Color;
import javafx.scene.paint.PhongMaterial;
import javafx.scene.shape.Circle;
import javafx.scene.shape.Line;
import javafx.scene.control.Tab;
import javafx.scene.shape.Rectangle;
import javafx.scene.shape.Shape;
import javafx.scene.control.MenuItem;
import java.util.Random;
import javafx.scene.paint.Paint;
import javafx.scene.layout.AnchorPane;
import javafx.event.ActionEvent;
import javafx.scene.shape.Box;

import javafx.scene.control.SplitMenuButton;
import javafx.scene.transform.Rotate;
import javafx.scene.transform.Translate;

public class HelloController {

//3 пр

    @FXML
    private AnchorPane pane3;
    @FXML
    private Tab tab3;
    @FXML
    private Box box;
    private Rotate rotateXAsis;
    private Rotate rotateYAsis;
    private Translate translate;
    private PhongMaterial material = new PhongMaterial();
    private final double movementSpeed = 10.0;
    private double mouseOldX, mouseOldY;
    private double mouseDeltaX, mouseDeltaY;
    private PerspectiveCamera camera;
    private final double rotationSpeed = 45.0;
    Group modelGroup = new Group();
    private final double mouseSensitivity = 0.1;
    //


    @FXML
    private Circle CIRCLE;

    @FXML
    private Line LINE;
    Double startMouseX;
    Double startMouseY;
    Double endMouseX;
    Double endMouseY;

    @FXML
    private Rectangle RECTANGLE;

    @FXML
    private Slider SLIDER;

    @FXML
    private MenuItem actionGREEN;
    private Paint color1;

    @FXML
    private MenuItem actionCIRCLE;

    @FXML
    private MenuItem actionLINE;

    @FXML
    private MenuItem actionRECTANGLE;

    @FXML
    private MenuItem actionRed;

    @FXML
    private MenuItem actionblue;

    @FXML
    private SplitMenuButton figura;
    @FXML
    private AnchorPane pane;

    @FXML
    private AnchorPane pane1;

    @FXML
    private SplitMenuButton svet;

    private double startAngle;

    private double anchorX;
    private double anchorY;

    Random random = new Random();

    private Color generateRandomColor(Random random) {
        return Color.rgb(random.nextInt(256), random.nextInt(256), random.nextInt(256));
    }

    // Исправляем название метода на OnMouseClicked (как в FXML)
    @FXML
    void OnMouseClicked(MouseEvent event) {
        Shape source = (Shape) event.getSource();

        if (event.getButton() == MouseButton.PRIMARY) {
            System.out.println(CIRCLE.getFill());

            CIRCLE.setFill(generateRandomColor(random));
        } else if (event.getButton() == MouseButton.SECONDARY) {
            CIRCLE.setFill(Paint.valueOf("radial-gradient(focus-angle 0.0deg, focus-distance 0.0% , center 50.0% 50.0%, radius 50.0%, 0xf2ff00ff 0.0%, 0xc90a0aff 100.0%)"));
        }
    }

    @FXML
    void figura(ActionEvent event) {

    }

    @FXML
    void actionMenuCIRCLE(ActionEvent event) {
        figura.setText("круг");
    }

    @FXML
    void actionMenuLINE(ActionEvent event) {
        figura.setText("линия");
    }

    @FXML
    void actionMenuRECTANGLE(ActionEvent event) {
        figura.setText("прямоугольник");
    }

    @FXML
    void actionRed(ActionEvent event) {
        svet.setText("красный");
        color1 = Color.RED;
    }

    @FXML
    void actionGreen(ActionEvent event) {
        svet.setText("зеленый");
        color1 = Color.GREEN;
    }

    @FXML
    void actionBlue(ActionEvent event) {
        svet.setText("синий");
        color1 = Color.BLUE;
    }

    @FXML
    void paneclick(MouseEvent event) {

    }

    @FXML
    void panedragged(MouseEvent event) {
        endMouseX = event.getX();
        endMouseY = event.getY();
        double currentWidth = SLIDER.getValue();
        switch (figura.getText()) {
            case "круг":
                double radius = Math.sqrt(Math.pow(startMouseX - endMouseX, 2) + Math.pow(startMouseY - endMouseY, 2));
                CIRCLE.setRadius(radius);
                break;

            case "линия":
                LINE.setEndX(endMouseX);
                LINE.setEndY(endMouseY);
                break;

            case "прямоугольник":
                RECTANGLE.setWidth(Math.abs(startMouseX - endMouseX));
                RECTANGLE.setHeight(Math.abs(startMouseY - endMouseY));
                RECTANGLE.setX(Math.min(startMouseX, endMouseX));
                RECTANGLE.setY(Math.min(startMouseY, endMouseY));
                break;
        }

    }

    @FXML
    void panepressed(MouseEvent event) {
        if (svet.getText().equals("Красный")) {
            color1 = Color.RED;
        } else if (svet.getText().equals("Зеленый")) {
            color1 = Color.GREEN;
        } else if (svet.getText().equals("Синий")) {
            color1 = Color.BLUE;
        } else {

        }
        startMouseX = event.getX();
        startMouseY = event.getY();

        switch (figura.getText()) {
            case "круг":
                CIRCLE = new Circle(startMouseX, startMouseY, 0);
                CIRCLE.setFill(color1);
                CIRCLE.setOnScroll(this::OnScroll);
                CIRCLE.setOnMouseClicked(this::OnMouseClicked);
                pane1.getChildren().add(CIRCLE);
                break;
            case "линия":
                System.out.println("линия");
                LINE = new Line(startMouseX, startMouseY, event.getX() + 1, event.getY() + 1);
                LINE.setStrokeWidth(SLIDER.getValue());
                LINE.setStroke(color1);
                LINE.setOnMousePressed(this::LineMousePressed);
                LINE.setOnMouseDragged(this::LineMouseDragged);
                LINE.setOnScroll(this::OnScroll);
                pane1.getChildren().add(LINE);
                break;

            case "прямоугольник":
                System.out.println("прямоугольник");
                RECTANGLE = new Rectangle(startMouseX, startMouseY, 20, 20);
                RECTANGLE.setFill(color1);
                RECTANGLE.setStroke(Color.BLACK);
                RECTANGLE.setStrokeWidth(SLIDER.getValue());
                RECTANGLE.setOnScroll(this::OnScroll);         // Подключаем ваш метод OnScroll
                RECTANGLE.setOnMouseMoved(this::MouseMoved);   // Подключаем ваш градиент
                RECTANGLE.setOnMouseExited(this::OnMouseExited);
                pane1.getChildren().addAll(RECTANGLE);
                break;
        }
    }


    @FXML
    void svet(ActionEvent event) {

    }


    // Исправляем название метода на OnScroll (как в FXML)
    @FXML
    void OnScroll(ScrollEvent event) {
        Object object = event.getSource(); // Определяем, на что навели мышку

        // Используем названия классов: Circle, Line, Rectangle (с большой буквы)
        if (object instanceof Circle) {
            if (event.getDeltaY() > 0) {
                CIRCLE.setRadius(CIRCLE.getRadius() + 5);
            } else {
                CIRCLE.setRadius(Math.max(1, CIRCLE.getRadius() - 5));
            }
        } else if (object instanceof Line) {
            if (event.getDeltaY() > 0) {
                // Исправлено: используем getStrokeWidth() для получения текущего значения
                LINE.setStrokeWidth(LINE.getStrokeWidth() * 1.05);
            } else {
                LINE.setStrokeWidth(LINE.getStrokeWidth() / 1.05);
            }
        } else if (object instanceof Rectangle) {
            if (event.getDeltaY() > 0) {
                // Исправлено: берем размеры у RECTANGLE, а не у LINE
                RECTANGLE.setWidth(RECTANGLE.getWidth() * 1.05);
                RECTANGLE.setHeight(RECTANGLE.getHeight() * 1.05);
            } else {
                RECTANGLE.setWidth(RECTANGLE.getWidth() / 1.05);
                RECTANGLE.setHeight(RECTANGLE.getHeight() / 1.05);
            }
        }
    }


    @FXML
    void OnMouseExited(MouseEvent event) {
        RECTANGLE.setFill(generateRandomColor(random));

    }

    @FXML
    void MouseMoved(MouseEvent event) {
        RECTANGLE.setFill(Paint.valueOf("linear-gradient(from 93.3649% 0.0% to 70.6161% 100.0%, #a15e51 0.0%, #18f500 100.0%)"));

    }

    @FXML
    void OnMouseDragged(MouseEvent event) {

    }

    @FXML
    void OnMousePressed(MouseEvent event) {

    }

    @FXML
    void LineMouseDragged(MouseEvent event) {
        double deltaX = event.getSceneX() - anchorX;
        double deltaY = event.getSceneY() - anchorY;
        double newAngle = Math.atan2(deltaY, deltaX) * 180 / Math.PI + 90;

        LINE.setRotate(startAngle + newAngle);

    }

    @FXML
    void LineMousePressed(MouseEvent event) {
        anchorX = event.getSceneX();
        anchorY = event.getSceneY();
        startAngle = LINE.getRotate();
    }

    //3 задание
    @FXML
    void actionpressedPane3(MouseEvent event) {
        if (event.getButton() == MouseButton.PRIMARY)
            mouseOldX = event.getSceneX();
        mouseOldY = event.getSceneY();
        System.out.println("press");
    }

    @FXML
    void pane3Dragged(MouseEvent event) {
        if (event.isPrimaryButtonDown()) {
            mouseDeltaX = event.getSceneX() - mouseOldX;
            mouseDeltaY = event.getSceneY() - mouseOldY;
            rotateXAsis.setAngle(rotateXAsis.getAngle() - mouseDeltaY * mouseSensitivity);
            rotateYAsis.setAngle(rotateYAsis.getAngle() + mouseDeltaX * mouseSensitivity);


            mouseOldX = event.getSceneX();
            mouseOldY = event.getSceneY();
            System.out.println("dragged");


        }
    }

    @FXML
    void scrollPane3(ScrollEvent event) {
        double delta = event.getDeltaY();
        if (delta > 0) {
            translate.setZ(translate.getZ() + movementSpeed);
        } else {
            translate.setZ(translate.getZ() - movementSpeed);
        }
        System.out.println("scroll");
    }

    public void box() {
        material.setDiffuseColor(Color.ORANGE);
        box.setMaterial(material);

        rotateXAsis = new Rotate(0, Rotate.X_AXIS);
        rotateYAsis = new Rotate(0, Rotate.Y_AXIS);
        translate = new Translate();

        // Применяем трансформации к кубу
        box.getTransforms().setAll(rotateXAsis, rotateYAsis, translate);

        // СОЗДАЕМ источники света (объявляем переменные)
        AmbientLight ambientLight = new AmbientLight(Color.rgb(100, 100, 100));

        PointLight pointLight = new PointLight(Color.WHITE);
        pointLight.setTranslateX(200);
        pointLight.setTranslateY(-200);
        pointLight.setTranslateZ(-400);

        // Очищаем и заново собираем группу
        modelGroup.getChildren().clear();
        modelGroup.getChildren().addAll(box, ambientLight, pointLight);

        // Добавляем группу на панель
        if (!pane3.getChildren().contains(modelGroup)) {
            pane3.getChildren().add(modelGroup);
        }

        modelGroup.setTranslateX(400);
        modelGroup.setTranslateY(300);
    }


    @FXML
    void initialize() {
        box();

    }
}
